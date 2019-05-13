---
title: "本地服务器搭建Harbor"
date: 2018-12-10T17:38:45+08:00
categories: ["All",“docker”,"Harbor","registry","helm","chart","k8s"]
tags: [“docker”,"Harbor","registry","helm","chart","k8s"]
toc: true
author: "Jermine & Grover"
author_homepage:  "/"
weight: 70
keywords: [“docker”,"Harbor","registry","helm","chart","k8s"]
description: "本地服务器搭建Harbor"
---

## Unix下安装Harbor镜像仓库

harbor的[官方安装指南](https://github.com/goharbor/harbor/blob/master/docs/installation_guide.md)介绍了harbor有三种安装方式，分别是在线安装、离线安装和OVA安装，本文主要采用离线安装的方式。

官方文档上面说明需要依赖Python 2.7或以上版本，Docker引擎1.10以上，还有Docker Compose 1.6.0或以上版本，openssl。

### 测试是否安装成功

Python: ```python -V```

Docker: ```docker version```

Docker Compose: ```docker-compose version```

 如未安装可参考文档：

[Docker Compose 参考文档](https://github.com/docker/compose/releases/)

[openssl 参考文档](https://www.cnblogs.com/blackhumour2018/p/9401765.html)

### 安装Harbor

#### 下载离线安装包
在[GitHub](https://github.com/goharbor/harbor/releases)上找到**Harbor offline installer**下载地址并下载，我这里下载的1.6.2版本。


#### 解压安装包
```
tar xvf harbor-offline-installer-v1.6.2.tar
```

#### 修改配置文件【harbor.cfg】
在*harbor*目录下，执行：

```
vi harbor.cfg

[root@sss-0004 harbor]# cat harbor.cfg 
## Configuration file of Harbor

#This attribute is for migrator to detect the version of the .cfg file, DO NOT MODIFY!
_version = 1.7.0
#The IP address or hostname to access admin UI and registry service.
#DO NOT use localhost or 127.0.0.1, because Harbor needs to be accessed by external clients.
#DO NOT comment out this line, modify the value of "hostname" directly, or the installation will fail.
hostname = hub.fi.vdo.pub

#The protocol for accessing the UI and token/notification service, by default it is http.
#It can be set to https if ssl is enabled on nginx.
ui_url_protocol = https

#Maximum number of job workers in job service  
max_job_workers = 10 

#Determine whether or not to generate certificate for the registry's token.
#If the value is on, the prepare script creates new root cert and private key 
#for generating token to access the registry. If the value is off the default key/cert will be used.
#This flag also controls the creation of the notary signer's cert.
customize_crt = on

#The path of cert and key files for nginx, they are applied only the protocol is set to https
ssl_cert = /data/harbor/cert/server.crt
ssl_cert_key = /data/harbor/cert/server.key

#The path of secretkey storage
secretkey_path = /data/harbor


```
必需的参数有：

1. **hostname**：目标主机的主机名，用于访问UI和注册服务。不能使用localhost和127.0.0.1，因为harbor需要被外部客户端访问，我这里修改成了IP地址192.168.16.85。
2. **ui\_url_protocol**：用于访问UI和令牌/通知服务的协议，默认为http，如果在Nginx上启用了SSL认证可以设置成https，我这里用的默认的https。
3. **max\_job_workers**：作业服务中的最大复制worker数，这里默认写的50，考虑到我的服务器的性能，我这里修改成了5。
4. **customize_crt**：设置为on，prepare脚本创建用于生成/验证注册表令牌的私钥和根证书。如果设置成off，密钥和根证书将由外部源提供，我设置的是on。
5. **ssl_cert**：SSL证书的位置，此处修改为**ssl_cert = /data/cert/cert.pem**，(后文会解释为什么)
6. **ssl_cert_key**：SSL秘钥的位置，此处修改为**sl\_cert_key = /data/cert/key.pem**，(同理)
7. **secretkey_path**：密码存放的路径，这里最好别修改，否则后面会报错.
8. **log_rotate_count**：日志文件保留的数量，达到最大值后会循环删除之前的日志。
9. **log_rotate_size**：每个日志的大小，我为了节省空间设置日志最多保留5个，每个最大200MB。
10. **db_password**：用于DB身份验证的MySQL数据库的根密码。

#### 获得SSL证书
执行：

```
openssl req -x509 -newkey rsa:4096 -keyout server.key -out server.crt -days 3000 -nodes
Generating a 4096 bit RSA private key
```

harbor文件夹下生成**cert.pem**和**key.pem**

#### 运行【prepare】更新参数
重新回到*harbor*，执行：

```
sudo ./prepare
```
> The configuration files are ready, please use docker-compose to start the survive.

#### 开始安装

执行：

```
./install.sh
```

> Harbor has been installed and started successfully

进行安装



#### 查看docker状态

```
docker ps -a
```

所有Status显示up xx seconds 说明安装成功，

访问**IP地址https://192.168.16.85**，使用admin账号登陆，搭建成功。

-------------------------

### 报错

 在安装的过程中可能会遇到各种报错，下面我把遇到的错误和解决办法总结一下，希望其他小伙伴能有所启发

#### 查看本地镜像列表
查看本地镜像列表：

```
docker images
```
如果为空，需要载入：

```
docker load -i harbor.v1.6.2.tar.gz
```
#### 权限问题
 
```
  ERROR: for registry  Cannot start service registry: b"error while creating mount source path '/Users/grover/harbor/common/config/custom-ca-bundle.crt': mkdir /Users/grover/harbor/common/config/custom-ca-bundle.crt: permission denied"
```
harbor的全部文件需要用户权限，我的harbor路径是 **/Users/grover/harbor** 

查看权限命令：

```
ls -la
```
```
ls -la common/
```
我发现**config**权限是**root**，那么执行：

```
sudo chown -R grover common
```
或

```
sudo chown -R grover common/config
```
把config权限改为普通用户(grover)

同理 **/data**下权限也是普通用户(grover)

```
sudo chown -R grover /data
```

#### Restarting问题
很多时候明明显示

```
----Harbor has been installed and started successfully.----
```
但是列出所有在运行的容器信息

```
docker ps -a
```
*goharbor/harbor-adminserver:v1.6.2* 和 *goharbor/registry-photon:v2.6.2-v1.6.2*一直在重启

删除所有已经停止的容器:

```
docker rm -f $(docker ps -aq)
```
删除 **/common/config**下所有文件：

```
rm -rf common/config/*
```
删除 **/data**下所有文件：

```
rm -rf /data/*
```

### 简化部署
为了简化上述流程，可以新建脚本：

```
cat > reset-harbor.sh 
#!/bin/bash
docker rm -f $(docker ps -aq -f "label=com.docker.compose.project=harbor" )
sudo rm -rf common/config/*
sudo rm -rf /data/harbor/*
sudo mkdir -p /data/harbor/cert && sudo cp crts/server.key /data/harbor/cert/server.key && sudo cp crts/server.crt /data/harbor/cert/server.crt
sudo ./prepare --with-notary --with-clair --with-chartmuseum
#sudo chown -R $(whoami) common/config
#sudo chown -R $(whoami) /data/harbor
sudo chmod -R 777 common/config
sudo chmod -R 777 /data/harbor
docker-compose -f ./docker-compose.yml -f ./docker-compose.notary.yml -f ./docker-compose.clair.yml -f ./docker-compose.chartmuseum.yml up -d
docker ps -a -f "label=com.docker.compose.project=harbor"
#每次./prepare和docker-compose up后都需要删除config和data里的文件。

```
退出执行：

```
chmod +x reset-harbor.sh
```

```
./reset-harbor.sh
```

## 其他搭建方式

### helm 安装：

参照Harbor官方给出的helm安装包: https://github.com/goharbor/harbor-helm
