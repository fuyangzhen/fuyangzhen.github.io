---
layout: post
title: "Build a docker private registry with Harbor"
tags: [tech, other]
date: 2018-07-24 00:00:00
comments: true
---  

`Harbor` 是由 `VMware` 中国研发团队负责开发的开源企业级 `Docker Registry`，不仅解决了我们直接使用 `Docker Registry`的功能缺失，更解决了我们在生产使用 `Docker Registry` 面临的高可用、镜像仓库直接复制、镜像仓库性能等运维痛点。在安装使用`Harbor`前，请先安装好`python2.7`,`docker-CE`和`docker-compose`。

1. 查看本机IP（inet）：`ip addr show eth0 `(也可以直接使用局域网内的域名，但是前提是域名要包含`.`，不然`docker push`会识别为官方的registry，即便你已经成功登录到这个private registry)

   <!--more--> 

2. 生成证书：

   ```bash
   # 先修改openssl配置
   sudo vi /etc/ssl/openssl.cnf
   # 找到[ v3_ca ]这一行，在下面加上：
   subjectAltName = IP:步骤1得到的IP地址
   
   mkdir -p certs && sudo openssl req \
   -newkey rsa:4096 -nodes -sha256 -keyout certs/ca.key \
   -x509 -days 365 -out certs/ca.crt
   # Common Name 填写第一步得到的IP地址，其他项无所谓
   ```

3. 全局使用刚生成的CA证书（**注意！需要全局重启docker的服务**）：  

   ```bash
   sudo cp certs/ca.crt /usr/local/share/ca-certificates/
   sudo update-ca-certificates
   sudo service docker restart
   ```
   其实有种方式可以不用重启`docker service`，但是官方解释这种方法只在部分版本的docker的下才能使用：

   > When using authentication, some versions of Docker also require you to trust the certificate at the OS level. 

   ```bash
   sudo mkdir -p /etc/docker/certs.d/第一步得到的IP地址/
   cp certs/ca.crt /etc/docker/certs.d/第一步得到的IP地址/
   ```

4. 安装启动[Harbor](https://github.com/vmware/harbor/releases) (当前版本号v1.5.2)：  

   ```bash
   wget https://storage.googleapis.com/harbor-releases/harbor-offline-installer-latest.tgz
   tar -zxvf harbor-offline-installer-latest.tgz
   cd harbor
   vi harbor.cfg
   
   # 修改harbor.cfg的几处配置：
   hostname = 步骤1得到的IP地址
   ui_url_protocol = ssl_cert = /path_to_your/certs/ca.crt
   ssl_cert_key = /path_to_your/certs/ca.key
   
   # 安装harbor：
   sudo ./install.sh
   ```

5. 局域网内作为registry hub的服务器设置完成了，内网的其他机器要想使用该registry就必须有步骤1中创建的`ca.crt`文件。

   ```bash
   scp ca.crt $USER@XXX.XXX.XXX.XXX:~
   sudo mkdir -p /etc/docker/certs.d/第一步得到的IP地址/
   cp certs/ca.crt /etc/docker/certs.d/第一步得到的IP地址/
   ```

6. 如何更改`harbor.cfg`并使其生效：

   ```bash
   sudo docker-compose down -v
   vim harbor.cfg
   sudo prepare
   # Removing Harbor's containers while keeping the image data and Harbor's database files on the file system
   sudo docker-compose up -d
   ```

7. 数据文件存放位置：  

   ````bash
   /data/registry # images
   /data/database # harbor 数据库
   /var/log/harbor/ # harbor 日志
   ````

   

Ps. 如果没有装python2.7到`usr/bin/python`，可以用`sudo apt install python2.7 python-pip`快速安装（也可以用软连接指到`usr/bin/python`）

```bash
# 安装Python2.7/usr/bin/python
sudo apt install python2.7 python-pip
# 安装docker-ce
sudo apt-get install docker-ce
# 安装docker-compose
sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# docker 免sudo
sudo chmod 666 /var/run/docker.sock
# docker 免sudo 另一种方式
sudo gpasswd -a $USER docker
newgrp - docker
# 还有一种方式
alias docker='sudo docker'

# python安装有问题，删除/usr/bin/python这个旧版本python
mv /usr/bin/python /usr/bin/python.old
rm -f /usr/bin/python-config
# 然后建立软链接
ln -s /usr/local/bin/python /usr/bin/python
ln -s /usr/local/bin/python-config /usr/bin/python-config
ln -s /usr/local/include/python2.7/ /usr/include/python2.7

# scp把crt分发出去
scp ca.crt $USER@XXX.XXX.XXX.XXX:~

# 新建一个linux用户
adduser $USER
# 为其添加sudo权限
sudo vi /etc/sudoers
# 加上一句
$USER ALL=(ALL:ALL) ALL
```

## migrate docker registry images



```bash
IDS=$(docker images | awk '{if ($1 ~ /^st.*/) print $1}') && docker save $IDS | bzip2 | ssh yanfu@10.190.178.161 "bunzip2 | docker load"管道
docker save hello-world | bzip2 | ssh root@10.140.1.120 "cat | docker load"

docker save $(docker images | awk '{if ($1 ~ /^carina.*/) printf $1" "}') | bzip2 > qna-images.tar.bz2

docker images | sed '1d' | awk '{if ($1 ~ /^carina.*/) print $1 " " $2 " " $3}' > qna-images.list
```




