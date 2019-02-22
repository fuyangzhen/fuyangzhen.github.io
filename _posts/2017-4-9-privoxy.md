---
layout: post
title: "OS X + Shadowsocks环境下的Privoxy的终端和py脚本内代理"
tags: [other, mac, OS X, Shadowsocks, Privoxy]
date: 2017-04-09T03:29:25-04:00
comments: true
---
***环境：OS X + Shadowsocks***  
***问题：虽然装有Shadowsocks，但是Twitter的pythonAPI仍无法使用***

在使用Twitter的pythonAPI时，发现在Shadowsocks开启的情况下，python的爬虫无法访问使用Twitter，此处整合了几个零散方法，分别在终端内全局代理和脚本内单独代理：

<!--more-->  
## 1 终端内代理  
方法来自[Fish](https://segmentfault.com/a/1190000008848001)  

* 下载安装[privoxy](http://www.privoxy.org)  
* 修改`/usr/local/etc/privoxy/config`路径下的配置文件：打开config文件，直接在最末尾加上  

```
listen-address 0.0.0.0:8118
forward-socks5 / 127.0.0.1:1080 .
```  
>1080是Shadowsocks代理的端口（也不一定是1080），8118是开启http代理的端口。使用0.0.0.0即可在局域网内使用此代理，如只想本机使用，使用127.0.0.1

* 打开终端，启动Privoxy：`sudo /usr/local/sbin/privoxy /usr/local/etc/privoxy/config`
* 验证是否成功，在终端中输入`netstat -an | grep 8118`，如出现如下结果表示已经成功:`tcp4 0 0 *.8118 *.* LISTEN`
* 终端内执行  

> export http_proxy='http://127.0.0.1:8118'  
> export https_proxy='http://127.0.0.1:8118'  

* 终端内输入` http www.youtube.com`测试代理效果
* 用完如需关闭，方法如下：`sudo /Applications/Privoxy/stopPrivoxy.sh`  

💡Ps.如果按此流程，爬虫仍然无法访问的话，可能是`config`配置文件仍有问题，可以点击下载 [我的config]({{ site.url }}/assets/Privoxy_config) 直接替换掉你电脑中上诉路径下Privoxy的`config`(注意:文件名字要修改保持为`config`不变)
## 2 py脚本内代理  
方法来自[techstay](https://segmentfault.com/q/1010000008986220):
> 第一种方式是使用Python3的urllib标准库。第二种是利用非常好用的requests库。headers必须加，因为不加谷歌返回来的不知道什么编码，没办法用UTF8解码  

```py
import urllib.request as request
import requests

proxies = {
    'https': 'https://127.0.0.1:1080',
    'http': 'http://127.0.0.1:1080'
}
headers = {
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36'
}

print('--------------使用urllib--------------')
google_url = 'https://www.google.com'
opener = request.build_opener(request.ProxyHandler(proxies))
request.install_opener(opener)

req = request.Request(google_url, headers=headers)
response = request.urlopen(req)

print(response.read().decode())

print('--------------使用requests--------------')
response = requests.get(google_url, proxies=proxies)
print(response.text)
```