---
title: "设置kvm中的虚拟机随宿主机自启动"
date: 2018-09-26T10:10:50+08:00
categories: ["All","server","linux"]
tags: ["kvm","server","linux"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["kvm","server","linux"]
description: "设置kvm中的虚拟机随宿主机自启动"
---

## 图形化方式

在kvm图形化管理工具里面可以设置，让kvm虚拟机随着宿主虚拟机一起启动。（**必须在关机状态下做**）

![image1](/img/kvm/28.png)

![image2](/img/kvm/29.png)


**设置好以后会像Windows一样创建一个快捷方式**

```
[root@CentOS2 ~]# cd /etc/libvirt/qemu/autostart/
[root@CentOS2 autostart]# ls
centos7.0.xml
```

**如果取消开机自动启动那个勾选，这个xml就不会被创建。**

## 终端控制台方式

以上是使用图形界面方式设置kvm虚拟机开机自动启动。下面演示命令行方式

```
[root@CentOS2 autostart]# virsh list --all
 Id    Name                           State
----------------------------------------------------
 -     centos7.0                      shut off
 -     winxp                          shut off

[root@CentOS2 autostart]# virsh autostart --disable centos7.0 # 取消自启动
Domain centos7.0 unmarked as autostarted

[root@CentOS2 autostart]# ls /etc/libvirt/qemu/autostart/
[root@CentOS2 autostart]# virsh autostart  centos7.0 # 设置自启动
Domain centos7.0 marked as autostarted

[root@CentOS2 autostart]# ls /etc/libvirt/qemu/autostart/
centos7.0.xml
```

