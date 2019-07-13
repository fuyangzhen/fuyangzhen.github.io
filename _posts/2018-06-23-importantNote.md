---
layout: post
title: "My Memo Note"
tags: [memo, other]
date: 2018-06-23 00:00:00
comments: true
---  



## C# Call RESTful  

```C#
public class RestAPICall
{
    public static string MakeCall(string method, string endpoint, string content, string contentType = "application/json")
    {
        string result = string.Empty;
        try
        {
            var request = (HttpWebRequest)WebRequest.Create(endpoint);
            request.ContentType = contentType;
            request.Method = method;
            request.Headers["authorization"] = "Basic";
            request.Headers["X-Anchormailbox"] = "ipat";
            request.Headers["X-CallerFileNameLin"] = "mytest.cs";
            request.Headers["X-ProcessName"] = "test.powershell.exe";
            request.Headers["X-ClientRequestId"] = "c03b981a";
            request.Headers["X-ActivityId"] = "c03b981a-3";
            request.Headers["X-CallerFileNameLine"] = "Mypowershell.ps1,line 3";

            //using (var streamWriter = new System.IO.StreamWriter(request.GetRequestStream()))
            //{
            //    string json = content;

            //    streamWriter.Write(json);
            //}

            var response = (HttpWebResponse)request.GetResponse();
            using (var streamReader = new System.IO.StreamReader(response.GetResponseStream()))
            {
                result = streamReader.ReadToEnd();
            }
        }
        catch (Exception e)
        {
            return e.ToString();
        }

        return result;
    }
}
```



## tmux 操作

"start": "webpack-dev-server --mode development --config ./webpack.dev.js --https --cert ./certs/server.crt --key ./certs/server.key", 

<!--more-->

## SQL server  

* copy table：`select * into CacheTemp from Cache`

* copy column: `update CacheTemp set ` 





## visual studio 快捷键

- 格式化：`ctrl + k -> ctrl + d`
- 注释： `ctrl + k -> ctrl + c `
- 取消注释： `ctrl + k -> ctrl + u `



## ASP.NET 命令行编译  

```power
dotnet publish -c Release -o out
dotnet out/XXX.dll
```

## git's best practice

1. `git stash`
2. ` git fetch origin master && git reset --hard origin/master`
3. `git stash pop`
4. `git commit -am "add&change blabla"`
5. `git push -u origin -f HEAD:user/yanfu`



- `git status`
- `git log --oneline --graph`
- `git push -f origin master`把远端master（新版本）强行改为本地master旧版本
- `git diff master..origin/master`对比远端和本地的分支不同之处
- `git checkout -- file_path`取消单文件的修改



如何在review的时候同步master代码同时修改commit:  

1. `git reset HEAD~1`  
2. `git stash`  
3. `git pull`  
4. `git stash pop`  
5. `git commit -am "bla bla"`  
6. `git push -u origin -f HEAD:user/yanfu`

## docker 相关操作

##### .netcore 项目的build  

dotnet publish -c Release -o out

##### build 、映射进入container内部、挂载外部文件夹

- `docker build -m 262144 -t es_rescore_with_obj:v0 .`

- `chown -R 1000:1000 path/to/data `

- `sudo docker run --privileged -p 9200:9200 --entrypoint "/bin/bash" -it es_rescore:1.0`

- `sudo docker run --privileged -p 9200:9200 -v /home/yanfu/make_docker_image/MakePureImage/data:/usr/share/elasticsearch/data -it es_rescore:2.0`

  ##### 彻底删除container

  先删container，才能彻底删除（docker rmi）image

- ```bash
  docker rm `docker ps -aq`
  ```

  ##### 为ES提升虚拟内存上线

  ```
  sudo sysctl -w vm.max_map_count=262144
  ```

  ##### push到Azure的docker仓库

- `docker tag es_rescore:2.0 skillbot.azurecr.io/es_rescore:v0`

- `docker login skillot.azurecr.io`

  - `username: skillot `
  - `paasword : 661EZGBW+ZZ5lSsWBjFgZZWh+IXjqgyxk`

- `docker push skillbot.azurecr.io/es_rescore:v0`

## iptables主机内部的端口重定向

我们可能需要将访问主机的7979端口映射到8080端口。也可以iptables重定向完成

```
iptables -t nat -A PREROUTING -p tcp --dport 7979 -j REDIRECT --to-ports 8080
```

##### 注意问题

需要打开ip_forward功能。

```
echo '1' > /proc/sys/net/ipv4/ip_forward  
```

## iptables跨网络、跨主机的映射Full-Nat

我们想到达主机B的80端口，但是由于网络限制可能无法直接完成。但是我们可以到达主机A的8080端口，而主机A可以直接到达B的80端口。
这时候可以使用iptables，将主机B的80端口映射到主机A的8080端口，通过访问A的8080相当于访问B的80。实现如下： 
在主机A上直接如下命令，实现端口映射的Full-Nat

```
#!/bin/bash
pro='tcp'
NAT_Host='Host_A'
NAT_Port=8080
Dst_Host='Host_B'
Dst_Port=80
iptables -t nat -A PREROUTING  -m $pro -p $pro --dport $NAT_Port -j DNAT --to-destination $Dst_Host:$Dst_Port
iptables -t nat -A POSTROUTING -m $pro -p $pro --dport $Dst_Port -d $Dst_Host -j SNAT --to-source $NAT_Host
```

说明：

1. NAT_Pro表示NAT的协议，可以是tcp或udp
2. NAT_Host表示中间做端口映射的主机。这里也就是主机A
3. NAT_Port表示中间做端口映射的端口。这里也就是主机A的8080口
4. Dst_Host表示被NAT的主机。这里也就是主机B
5. Dst_Host表示被NAT的端口。这里也就是主机B的80口

## 配置 Nginx的端口重定向（复杂，可以直接用iptables）

```
$ sudo /etc/init.d/nginx start	#启动nginx

$ sudo rm /etc/nginx/sites-enabled/default	#删除默认配置
$ sudo touch /etc/nginx/sites-available/test	#建立项目文件
$ sudo ln -s /etc/nginx/sites-available/test /etc/nginx/sites-enabled/test	#设置软链接

$ sudo vim /etc/nginx/sites-enabled/test	#编辑项目文件
```

添加：

```
server {
    location / {
        proxy_pass http://127.0.0.1:8000;
    }
}
```

当HTTP请求得到`/`，便会反向代理到`127.0.0.1`8000端口。 重启 nginx:

```
$ sudo /etc/init.d/nginx restart
```

## sudo 情景下无法识别部分命令

把`psudo() { sudo env PATH="$PATH" "$@"; } `加入到~/.bashrc，然后使用`psudo`

## netstat

`netstat -anp|grep 80 `or`lsof -i:端口号 `查看端口占用情况





