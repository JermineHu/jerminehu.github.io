---
title: "Redhat7解决This system is not registered to Red Hat Subscription Management."
date: 2018-09-05T14:38:41+08:00
categories: ["All","Linux"]
tags: ["Linux",]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Redhat7","Linux"]
description: "Redhat7解决This system is not registered to Red Hat Subscription Management."
---

## 错误描述：

```
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register. 
```

## 解决步骤:

### 删除redhat原有的yum 
 
```
rpm -aq|grep yum|xargs rpm -e --nodeps 

```
### 下载yum安装文件

到这个网站去下载如下RPM包 ：http://mirrors.163.com/centos/7/os/x86_64/Packages/

```

wget http://mirrors.163.com/centos/7/os/x86_64/Packages/yum-3.4.3-158.el7.centos.noarch.rpm
wget http://mirrors.163.com/centos/7/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
wget http://mirrors.163.com/centos/7/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.31-45.el7.noarch.rpm
wget http://mirrors.163.com/centos/7/os/x86_64/Packages/python-iniparse-0.4-9.el7.noarch.rpm

``` 

### 进行安装yum 

```
rpm -ivh --force --nodeps python-iniparse-0.4-9.el7.noarch.rpm
rpm -ivh --force --nodeps yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
rpm -ivh --force --nodeps yum-3.4.3-158.el7.centos.noarch.rpm yum-plugin-fastestmirror-1.1.31-45.el7.noarch.rpm


```

_注意:最后两个包必需同时安装，否则会相互依赖 _

### 设置源

```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo  

```

### 清除原有缓存

```
yum clean all

```

### 重建缓存，以提高搜索安装软件的速度

```
yum makecache

```
如果有提示错误，运行下面的命令：`[Errno 14] PYCURL ERROR 22 - "The requested URL returned error: 404 Not Found"`
`sed -i 's/\$releasever/7/' CentOS-Base.repo ` 备注：把文件里的`$releasever`替换为7
最后：重新生成缓存，数字不为0，就OK了：
```
yum clean all
yum makecache

```

### 更新系统

```
yum update
```