---
title: "管理kvm常用命令总结"
date: 2018-09-26T10:18:25+08:00
categories: ["All","server","linux"]
tags: ["kvm","server","linux"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["kvm","server","linux"]
description: "管理kvm常用命令总结"
---


## 查看虚拟机列表及状态

```
[root@kvm01 ~]# virsh list --all 
Id    Name                           State
---------------------------------------------------- 
-     vm1                            shut off
```

## 启动虚拟机系统

```
[root@kvm01 ~]# virsh start vm1
Domain vm1 started
```

## 停止虚拟机系统

```
[root@kvm01 ~]# virsh shutdown vm1
```

停止虚拟机要求虚拟机开启acpid服务

## 重启虚拟机系统
```
[root@kvm01 ~]# virsh reboot vm1
```
## 强制关机虚拟机系统

```
[root@kvm01 ~]# virsh destroy vm1
```
## 设置虚拟机随宿主机开机自启

```
[root@kvm01 ~]# virsh autostart vm1
```
## 取消虚拟机随宿主机开机自启

```
[root@kvm01 ~]# virsh autostart --disable vm1
```
## 挂起及恢复虚拟机
**挂起：**
```
[root@kvm01 ~]# virsh suspend vm1
```
**恢复：**

```
[root@kvm01 ~]# virsh resume vm1
```
## 编辑虚拟机XML配置文件

```
[root@kvm01 ~]# virsh edit vm1
```

注：vm1配置文件要求已经define

## 定义虚拟机XML配置文件
修改了虚拟机XML配置文件以后要求声明XML配置文件

```
[root@kvm01 ~]# virsh define /etc/libvirt/qemu/vm1.xml
```

或声明XML配置文件，并启动虚拟机

```
[root@kvm01 ~]# virsh create /etc/libvirt/qemu/vm1.xml
```

## 取消声明的虚拟机XML配置文件

```
[root@kvm01 ~]# virsh undefine vm1
```

## 创建虚拟机


### 准备操作系统的镜像文件

上传ISO文件到/data/iso下，这里使用CentOS-5.5-i386-bin-DVD.iso

### 安装CentOS5.5
CentOS7.1 安装KVM虚拟机默认磁盘格式为qcow2（推荐使用，空间动态增长）。
#### 命令行安装：
```
virt-install --network bridge=br0,model=virtio --name vm1 --ram=768 --vcpus=1 --disk path=/vm-images/vm1.img,size=10,bus=virtio --cdrom /data/iso/CentOS-5.5-i386-bin-DVD.iso --graphics none --noautoconsole  --accelerate
```
#### 用Virtual Machine Manager连接完成安装

操作如下:

![image1](/img/kvm/1.png)


文本模式完成CentOS5.5系统最小化安装（过程中加入内核参数console=tty0 console=ttyS0,115200），关闭iptables、selinux，修改主机名为vm1.ylhb.com，查看系统是否默认开启acpid服务，如无则启动，并设为开机自启,以上即完成linux虚拟机的安装。


### 登入虚拟机

#### 方法一：
如上采用Virtual Machine Manager，选择虚拟机打开即可

![image2](/img/kvm/2.png)
![image3](/img/kvm/3.png)

#### 方法二：
使用CRT、Xshell等终端工具连接，具体怎么使用，这里不再叙述

#### 方法三：

在KVM01上，`virsh console XX`进入vm1虚拟机控制台，如下:

```
[root@kvm01 ~]# virsh console vm1
Connected to domain vm1Escape character is ^] 
CentOS release 5.5 (Final)
Kernel 2.6.18-194.el5 on an i686 
localhost login: root
Password: 
Last login: Fri Mar 11 10:22:34 on tty1
[root@vm1~]# 
```

退出控制台按`ctrl + ]`


## 删除虚拟机
1.关闭虚拟机系统

```
[root@kvm01 ~]# virsh shutdown vm1
```

若不生效则强制关机

```
[root@kvm01 ~]# virsh destroy vm1
```

2.取消开机自启
```
[root@kvm01 ~]# virsh autostart --disable vm1
```

3.取消虚拟机XML配置文件定义
```
[root@kvm01 ~]# virsh undefine vm1
```
4.删除虚拟机磁盘文件
```
rm -rf /vm-images/vm1.img
```
## 备份(导出)虚拟机XML配置文件
```
virsh dumpxml vm1 > /etc/libvirt/qemu/vm1_dump.xml
```
## 控制台管理虚拟机
```
[root@kvm01 ~]# virsh console vm1
```
## 显示虚拟机信息
```
virsh dominfo vm1
```

## 查看磁盘信息
```
[root@kvm01 ~]# qemu-img info /vm-images/vm1.img 
image: /vm-images/vm1.img
file format: qcow2
virtual size: 10G (10737418240 bytes)
disk size: 1.7G
cluster_size: 65536
Format specific information:
    compat: 1.1    
    lazy refcounts: true
```