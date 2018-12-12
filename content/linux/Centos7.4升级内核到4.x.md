---
title: "Centos7.4升级内核到4"
date: 2016-12-12T18:08:28+08:00
categories: ["All","Linux"]
tags: ["Linux"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Linux","Centos" ]
description: "Centos7.4升级内核到4"
---


## 查看内核版本：uname -r

```
[root@k8s-node02 ~]# uname -r
3.10.0-862.9.1.el7.x86_64
[root@k8s-node02 ~]# 

```

内核版本为3.10.0

## 导入elrepo的key，然后安装elrepo的yum源

```

rpm -import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org

rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm

```

使用以下命令列出可用的内核相关包

```
yum --disablerepo="*" --enablerepo="elrepo-kernel" list available

```

```
[root@localhost ~]# yum --disablerepo="*" --enablerepo="elrepo-kernel" list available
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * elrepo-kernel: hkg.mirror.rackspace.com
Available Packages
kernel-lt.x86_64                                                  4.4.166-1.el7.elrepo                                 elrepo-kernel
kernel-lt-devel.x86_64                                            4.4.166-1.el7.elrepo                                 elrepo-kernel
kernel-lt-doc.noarch                                              4.4.166-1.el7.elrepo                                 elrepo-kernel
kernel-lt-headers.x86_64                                          4.4.166-1.el7.elrepo                                 elrepo-kernel
kernel-lt-tools.x86_64                                            4.4.166-1.el7.elrepo                                 elrepo-kernel
kernel-lt-tools-libs.x86_64                                       4.4.166-1.el7.elrepo                                 elrepo-kernel
kernel-lt-tools-libs-devel.x86_64                                 4.4.166-1.el7.elrepo                                 elrepo-kernel
kernel-ml-doc.noarch                                              4.19.8-1.el7.elrepo                                  elrepo-kernel
kernel-ml-headers.x86_64                                          4.19.8-1.el7.elrepo                                  elrepo-kernel
kernel-ml-tools.x86_64                                            4.19.8-1.el7.elrepo                                  elrepo-kernel
kernel-ml-tools-libs.x86_64                                       4.19.8-1.el7.elrepo                                  elrepo-kernel
kernel-ml-tools-libs-devel.x86_64                                 4.19.8-1.el7.elrepo                                  elrepo-kernel
perf.x86_64                                                       4.19.8-1.el7.elrepo                                  elrepo-kernel
python-perf.x86_64                                                4.19.8-1.el7.elrepo                                  elrepo-kernel
[root@localhost ~]# 

```



可以看出，长期维护版本lt为4.4，最新主线稳定版ml为4.19，我们需要安装最新的主线稳定内核，使用如下命令：

yum -y --enablerepo=elrepo-kernel install kernel-ml.x86_64 kernel-ml-devel.x86_64

## 查看内核版本默认启动顺序：

```
awk -F\' '$1=="menuentry " {print $2}' /etc/grub2.cfg
```
版本内核目前位置为0，原来的3.10版本内核目前位置为1，所以需要修改内核启动顺序为0。

## 修改grub中默认版本启动顺序：

运行grub2-mkconfig命令来重新创建内核配置：

```
grub2-set-default 0
grub2-mkconfig -o /boot/grub2/grub.cfg
```

重启：reboot

待启动完毕，查看系统内核：uname -r

```
[root@hub ~]# uname -r
4.19.8-1.el7.elrepo.x86_64
[root@hub ~]# 
```
内核版本已升级为4.19
--------------------- 
