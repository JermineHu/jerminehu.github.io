---
title: "LDAP与Harbor集成"
date: 2018-09-19T13:56:48+08:00
categories: ["All","DevOps","Linux"]
tags: ["Devops","LDAP","Harbor","Linux"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Devops","LDAP","Harbor","Linux"]
description: "LDAP与Harbor集成"
---
# LDAP与Harbor集成

###### OpenLDAP是一个集中的用户账号管理系统；使用轻量级目录访问协议（LDAP）构建集中的身份验证系统可以减少管理成本，增强安全性，避免数据复制的问题，并提高数据的一致性。
## 修改harbor.cfg

Harbor默认是使用mysql数据库进行用户管理，那么我们需要修改Harbor的配置文件。

在*harbor*目录下，执行：

```
vi harbor.cfg
```
1. **auth_mode = ldap_auth** ：验证模式
2. **ldap_url = ldap://192.168.16.196**：openldap  server
3. **ldap_basedn = ou=dev,dc=xinktech,dc=com**：同上
4. **ldap_filter = (objectClass=person)**：默认
5. **ldap_group_basedn = ou=dev,dc=xinktech,dc=com**
6. **ldap_group_filter = objectclass=posixGroup**

## 生成自签名证书
首先，把自己的IP地址(192.168.16.85)域名设置为: test.harbor.com:

```
sudo vim /etc/hosts
```
在最后一行添加：

```
192.168.16.85   test.harbor.com
```

在harbor目录下，执行：
	
```
openssl req \
    -newkey rsa:4096 -nodes -sha256 -keyout server.key \
-x509 -days 365 -out server.crt
```
打开harbor文件夹，发现生成**server.key**和**server.crt**

```
openssl req \
    -newkey rsa:4096 -nodes -sha256 -keyout test.harbor.com.key \
    -out test.harbor.com.csr
```
再次打开harbor文件夹，生成了**test.harbor.csr**和**test.harbor.key**

新建demoCA文件夹

```
mkdir ./demoCA
```
```
touch demoCA/index.txt
```
```
echo '01' > demoCA/serial
```
```
openssl ca -in test.harbor.com.csr -out test.harbor.com.crt -cert server.crt -keyfile server.key -outdir
```
打开harbor文件夹，新生成了**test.harbor.com.crt**。

把**test.harbor.com.crt**和**test.harbor.com.key**移动到/data/cert里，并重新命名为**server.crt**和**server.key**

```
cp test.harbor.com.crt /data/cert
```
```
cp test.harbor.com.key /data/cert
```
```
mv test.harbor.com.crt server.crt
```
```
mv test.harbor.com.key server.key
```
## 重新启动

```
sudo ./prepare
```

```
sudo chown -R $(whoami) common/config
```

```
sudo chown -R $(whoami) /data
```

```
docker-compose up -d
```

如果harbor-adminserver一直restart，执行：

```
docker restart [IMAGE ID]
```
## 登陆前的准备

在登陆docker
```
docker login test.harbor.com
```
时会报错：

```
Error response from daemon: Get https://192.168.16.85/v2/: x509: cannot validate certificate for 192.168.16.85 because it doesn't contain any IP SANs
```
登陆 **https://test.harbor.com**，用户名：**admin**，密码：**Harbor12345**（默认值，可在**harbor.cfg**文件中修改）

LDAP URL：**ldap://192.168.16.196**

LDAP搜索DN修改为：**cn=admin,dc=com** 
###### 一直写的是**uid=Jermine,ou=dev,dc=xinktech,dc=com**，这是不对的，因为Jermine只有用户权限，不是管理员。

LDAP搜索密码：**123456**

LDAP基础DN：**ou=dev,dc=xinktech,cn=com**

LDAP搜索范围：**子树**

LDAP组基础DN：**ou=dev,dc=xinktech,dc=com**

LDAP组过滤器：**objectclass=posixGroup**

## pull&&push

登陆用户**cgh**

```
docker images
```
```
REPOSITORY                      TAG                 IMAGE ID            CREATED             SIZE
goharbor/chartmuseum-photon     v0.7.1-v1.6.2       00d802f6d22d        11 days ago         350MB
goharbor/harbor-migrator        v1.6.2              3b6080f235a1        11 days ago         799MB
goharbor/redis-photon           v1.6.2              473bfdd9d245        11 days ago         210MB
goharbor/clair-photon           v2.0.6-v1.6.2       1bdd82973b10        11 days ago         302MB
goharbor/notary-server-photon   v0.5.1-v1.6.2       7b98dbd05547        11 days ago         209MB
goharbor/notary-signer-photon   v0.5.1-v1.6.2       e32e45d2c4b9        11 days ago         206MB
goharbor/registry-photon        v2.6.2-v1.6.2       62c30cdb384a        11 days ago         196MB
goharbor/nginx-photon           v1.6.2              c0602500e829        11 days ago         132MB
goharbor/harbor-log             v1.6.2              781ee4ceb5d3        11 days ago         197MB
goharbor/harbor-jobservice      v1.6.2              3419a2276f96        11 days ago         192MB
goharbor/harbor-ui              v1.6.2              66268686bb96        11 days ago         215MB
goharbor/harbor-adminserver     v1.6.2              4024440925a4        11 days ago         181MB
goharbor/harbor-db              v1.6.2              0ed4186be0d1        11 days ago         219MB
```

```
docker pull test.harbor.com/alpine/alpine:test
```
```
REPOSITORY                      TAG                 IMAGE ID            CREATED             SIZE
goharbor/chartmuseum-photon     v0.7.1-v1.6.2       00d802f6d22d        11 days ago         350MB
goharbor/harbor-migrator        v1.6.2              3b6080f235a1        11 days ago         799MB
goharbor/redis-photon           v1.6.2              473bfdd9d245        11 days ago         210MB
goharbor/clair-photon           v2.0.6-v1.6.2       1bdd82973b10        11 days ago         302MB
goharbor/notary-server-photon   v0.5.1-v1.6.2       7b98dbd05547        11 days ago         209MB
goharbor/notary-signer-photon   v0.5.1-v1.6.2       e32e45d2c4b9        11 days ago         206MB
goharbor/registry-photon        v2.6.2-v1.6.2       62c30cdb384a        11 days ago         196MB
goharbor/nginx-photon           v1.6.2              c0602500e829        11 days ago         132MB
goharbor/harbor-log             v1.6.2              781ee4ceb5d3        11 days ago         197MB
goharbor/harbor-jobservice      v1.6.2              3419a2276f96        11 days ago         192MB
goharbor/harbor-ui              v1.6.2              66268686bb96        11 days ago         215MB
goharbor/harbor-adminserver     v1.6.2              4024440925a4        11 days ago         181MB
goharbor/harbor-db              v1.6.2              0ed4186be0d1        11 days ago         219MB
test.harbor.com/alpine/alpine   test                196d12cf6ab1        2 months ago        4.41MB
```

```
docker rmi 196
```
```
docker images
```
```
REPOSITORY                      TAG                 IMAGE ID            CREATED             SIZE
goharbor/chartmuseum-photon     v0.7.1-v1.6.2       00d802f6d22d        11 days ago         350MB
goharbor/harbor-migrator        v1.6.2              3b6080f235a1        11 days ago         799MB
goharbor/redis-photon           v1.6.2              473bfdd9d245        11 days ago         210MB
goharbor/clair-photon           v2.0.6-v1.6.2       1bdd82973b10        11 days ago         302MB
goharbor/notary-server-photon   v0.5.1-v1.6.2       7b98dbd05547        11 days ago         209MB
goharbor/notary-signer-photon   v0.5.1-v1.6.2       e32e45d2c4b9        11 days ago         206MB
goharbor/registry-photon        v2.6.2-v1.6.2       62c30cdb384a        11 days ago         196MB
goharbor/nginx-photon           v1.6.2              c0602500e829        11 days ago         132MB
goharbor/harbor-log             v1.6.2              781ee4ceb5d3        11 days ago         197MB
goharbor/harbor-jobservice      v1.6.2              3419a2276f96        11 days ago         192MB
goharbor/harbor-ui              v1.6.2              66268686bb96        11 days ago         215MB
goharbor/harbor-adminserver     v1.6.2              4024440925a4        11 days ago         181MB
goharbor/harbor-db              v1.6.2              0ed4186be0d1        11 days ago         219MB
```
退出**cgh**账户

```
docker logout test.harbor.com
```
```
docker pull test.harbor.com/alpine/alpine:test
```
因为项目默认是公开的，所以也可以pull。

```
test: Pulling from alpine/alpine
4fe2ade4980c: Pull complete 
Digest: sha256:02892826401a9d18f0ea01f8a2f35d328ef039db4e1edcc45c630314a0457d5b
Status: Downloaded newer image for test.harbor.com/alpine/alpine:test
```

修改项目权限，再次pull发现：

```
Error response from daemon: pull access denied for test.harbor.com/alpine/alpine, repository does not exist or may require 'docker login'
```

再次登陆**cgh**

```
docker pull test.harbor.com/alpine/alpine:test
```


```
test: Pulling from alpine/alpine
4fe2ade4980c: Pull complete 
Digest: sha256:02892826401a9d18f0ea01f8a2f35d328ef039db4e1edcc45c630314a0457d5b
Status: Downloaded newer image for test.harbor.com/alpine/alpine:test
```
又可以pull了，因为赋予了cgh权限。将其删除后登出**cgh**

登陆**Jermine**账号

```
docker login test.harbor.com
```
```
docker pull test.harbor.com/alpine/alpine:test
```
```
Error response from daemon: pull access denied for test.harbor.com/alpine/alpine, repository does not exist or may require 'docker login'
```

发现报错，Jermine没有权限，因为项目没有添加Jermine账户管理权限。

添加后再次pull

```
docker pull test.harbor.com/alpine/alpine:test
```
```
test: Pulling from alpine/alpine
4fe2ade4980c: Pull complete 
Digest: sha256:02892826401a9d18f0ea01f8a2f35d328ef039db4e1edcc45c630314a0457d5b
Status: Downloaded newer image for test.harbor.com/alpine/alpine:test
```
成功了

