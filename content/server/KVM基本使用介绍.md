---
title: "KVM基本使用介绍"
date: 2018-09-26T11:27:37+08:00
categories: ["All","server","linux"]
tags: ["kvm","server","linux"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["kvm","server","linux"]
description: "KVM基本使用介绍"
---

## KVM是什么？

KVM 全称是 基于内核的虚拟机(Kernel-based Virtual Machine)，它是一个 Linux 的一个内核模块，该内核模块使得 Linux 变成了一个 Hypervisor：

* 它由 Quramnet 开发，该公司于 2008年被 Red Hat 收购。
* 它支持 x86 (32 and 64 位), s390, Powerpc 等 CPU。
* 它从 Linux 2.6.20 起就作为一模块被包含在 Linux 内核中。
* 它需要支持虚拟化扩展的 CPU。
* 它是完全开源的。
 
### KVM架构
　　KVM 是基于虚拟化扩展(Intel VT 或者 AMD-V)的 X86 硬件的开源的 Linux 原生的全虚拟化解决方案。KVM 中，虚拟机被实现为常规的 Linux 进程，由标准 Linux 调度程序进行调度;虚机的每个虚拟 CPU 被实现为一个常规的 Linux 进程。这使得 KMV 能够使用 Linux 内核的已有功能。

　　但是，KVM 本身不执行任何硬件模拟，需要客户空间程序通过 /dev/kvm 接口设置一个客户机虚拟服务器的地址空间，向它提供模拟的 I/O，并将它的视频显示映射回宿主的显示屏。目前这个应用程序是 QEMU。

　　Linux 上的用户空间、内核空间和虚机：

![image4](/img/kvm/4.jpg)

* Guest：客户机系统，包括CPU（vCPU）、内存、驱动（Console、网卡、I/O 设备驱动等），被 KVM 置于一种受限制的 CPU 模式下运行。
* KVM：运行在内核空间，提供CPU 和内存的虚级化，以及客户机的 I/O 拦截。Guest 的 I/O 被 KVM 拦截后，交给 QEMU 处理。
* QEMU：修改过的为 KVM 虚机使用的 QEMU 代码，运行在用户空间，提供硬件 I/O 虚拟化，通过 IOCTL /dev/kvm 设备和 KVM 交互。


### KVM功能

**KVM 所支持的功能包括：**

* 支持CPU 和 memory 超分(Overcommit)
* 支持半虚拟化I/O (virtio)
* 支持热插拔 (cpu，块设备、网络设备等)
* 支持对称多处理(Symmetric Multi-Processing，缩写为 SMP )
* 支持实时迁移(Live Migration)
* 支持 PCI 设备直接分配和 单根I/O 虚拟化 (SR-IOV)
* 支持 内核同页合并 (KSM )
* 支持 NUMA (Non-Uniform Memory Access，非一致存储访问结构 )

### KVM常用工具
* libvirt：操作和管理KVM虚机的虚拟化 API，使用 C 语言编写，可以由 Python,Ruby, Perl, PHP, Java 等语言调用。可以操作包括 KVM，vmware，XEN，Hyper-v, LXC 等 Hypervisor。
* Virsh：基于 libvirt 的 命令行工具 (CLI)
* Virt-Manager：基于 libvirt 的 GUI 工具
* virt-v2v：虚机格式迁移工具
* virt-* 工具：包括 Virt-install (创建KVM虚机的命令行工具)， Virt-viewer (连接到虚机屏幕的工具)，Virt-clone(虚机克隆工具)，virt-top 等
* sVirt：安全工具
  
## KVM安装

安装前要查看CPU是否支持虚拟化
Lntel CPU:
```
[root@localhost ~]# cat /proc/cpuinfo |grep vmx
```
![image5](/img/kvm/5.png)


若以上操作有输出，就说明CPU支持虚拟化
 
### 实验环境：

    KVM:Centos6.5 64位操作系统
    内存4GB
    硬盘20G
    开启CPU虚拟化支持：

 ![image6](/img/kvm/6.png)

**安装依赖关系:**
```
[root@localhost ~]# yum -y install qemu-kvm qemu-kvm-tools python-virtinst.noarch qemu-img bridge-utils libvirt virt-manager
```

 ![image7](/img/kvm/7.png)

**查看是否加载了KVM模块**

```
[root@localhost ~]# lsmod | grep kvm
kvm_intel              54285  0
kvm                   333172  1 kvm_intel
```

在libvirt中运行KVM网络有两种方法：NAT和bridge，默认是NAT。
将以bridge（桥接模式）为例。

```
[root@localhost ~]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue state UNKNOWN
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 00:0c:29:88:85:64 brd ff:ff:ff:ff:ff:ff
    inet 192.168.2.113/24 brd 192.168.2.255 scope global eth0
    inet6 fe80::20c:29ff:fe88:8564/64 scope link
       valid_lft forever preferred_lft forever
3: pan0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN
link/ether c2:34:e0:1c:77:37 brd ff:ff:ff:ff:ff:ff
 
[root@localhost ~]# cd /etc/sysconfig/network-scripts/
[root@localhost network-scripts]# cp ifcfg-eth0 ifcfg-br0
 
 
[root@localhost network-scripts]# vim ifcfg-br0
DEVICE=br0
HWADDR=00:0c:29:88:85:64
TYPE=Bridge
ONBOOT=yes
NM_CONTROLLED=no
BOOTPROTO=static
IPADDR=192.168.2.113
NETMASK=255.255.255.0
 
[root@localhost network-scripts]# vim ifcfg-eth0
 
DEVICE=eth0
HWADDR=00:0c:29:88:85:64
TYPE=Ethernet
ONBOOT=yes
NM_CONTROLLED=no
BOOTPROTO=static
BRIDGE="br0"  #桥接虚拟网卡名称
```

```
[root@localhost ~]# /etc/init.d/network restart
```

![image8](/img/kvm/8.png)

```
[root@localhost ~]# ip a
```
![image9](/img/kvm/9.png)
  
## virt-manager的使用

```
[root@localhost ~]# mkdir -pv /data_kvm/{store,iso}
mkdir: 已创建目录 "/data_kvm"
mkdir: 已创建目录 "/data_kvm/store"
mkdir: 已创建目录 "/data_kvm/iso"
```

### 进入图形界面
```
[root@localhost ~]# virt-manager
```
连接出错，重启`libvirtd`就好了
 


![KVM](/img/kvm/10.png)

 
**重启libvirtd**

```
[root@localhost ~]# /etc/init.d/libvirtd  start
[root@localhost ~]# virt-manager
 ```

![KVM](/img/kvm/11.png)

 
### 添加池
双击localhost{QEMU}----存储-----“+”添加池


![KVM](/img/kvm/12.png)

![KVM](/img/kvm/13.png)

选择浏览-----找到刚才创建的目录------完成
 
![KVM](/img/kvm/14.png)

 
### 创建存储卷
单机刚创建的KVM01池----新建卷
 
![KVM](/img/kvm/14.png)

![KVM](/img/kvm/15.png)

![KVM](/img/kvm/16.png)


 
### 新建虚拟机



新建虚拟机

![KVM](/img/kvm/17.png)
![KVM](/img/kvm/18.png)
![KVM](/img/kvm/19.png)
![KVM](/img/kvm/20.png)
![KVM](/img/kvm/21.png)
![KVM](/img/kvm/22.png)



具体安装过程与安装Linux系统一样
 
![KVM](/img/kvm/23.png)

 
查看KVM的配置文件存放目录

```
[root@localhost ~]# ls /etc/libvirt/qemu
centos6.5.xml  networks
```

查看虚拟机状态
```
[root@localhost ~]# virsh list --all
 Id    名称                         状态
----------------------------------------------------
 2     centos6.5                      running（开启）
```

虚拟机的关机、开机自启等操作
要保证acpid服务安装并运行

```
[root@localhost ~]# yum -y install acpid
```
![KVM](/img/kvm/24.png)
 
```
[root@localhost ~]# /etc/init.d/acpid start
 
[root@localhost ~]# /etc/init.d/haldaemon stop
正在关闭 HAL 守护进程：                                    [确定]
 
[root@localhost ~]# /etc/init.d/acpid start
[root@localhost ~]# /etc/init.d/haldaemon start
启动 HAL 守护进程：                                        [确定]
 
[root@localhost ~]# /etc/init.d/acpid status
acpid (pid  1417) 正在运行...
 
关机KVM虚拟机
[root@localhost ~]# virsh shutdown centos6.5
域 centos6.5 被关闭

```

```
[root@localhost ~]# virsh destroy centos6.5
域 centos6.5 被删除
```

![KVM](/img/kvm/25.png)


```
[root@localhost ~]# virsh list --all
 Id    名称                         状态
----------------------------------------------------
 -     centos6.5                      关闭
```


### 开机KVM虚拟机

```
[root@localhost ~]# virsh start centos6.5
域 centos6.5 已开始
 ```

![KVM](/img/kvm/26.png)
 
 ```
[root@localhost ~]# virsh list --all
 Id    名称                         状态
----------------------------------------------------
 4     centos6.5                      running
```

### 虚拟机伴随宿主机自动启动
```
[root@localhost ~]# virsh autostart centos6.5
域 centos6.5标记为自动开始
 
[root@localhost ~]# ls /etc/libvirt/qemu
autostart  centos6.5.xml  networks
```

### 导出虚拟机配置

```
[root@localhost ~]# virsh dumpxml centos6.5 > /etc/libvirt/qemu/centos6.5_bak.xml
``` 

### 删除虚拟机

```
[root@localhost ~]# virsh undefine centos6.5
```

### 修改虚拟机配置信息

```
[root@localhost ~]# virsh edit centos6.5
``` 

## KVM文件管理，raw格式转换为qcow2格式

虚拟机磁盘文件分为raw与qcow2格式，KVM默认格式是raw裸设备。
raw好处：性能好、速度最快。缺点：不支持一些新的功能。如：镜像、zlib磁盘压缩，AES加密等
 
libguestfs-tools工具实现格式转换
```
[root@localhost ~]# yum -y install libguestfs-tools
```
 
 ![KVM](/img/kvm/27.png)

```
[root@localhost ~]# qemu-img info /data_kvm/store/KVM011.img
image: /data_kvm/store/KVM011.img
file format: raw
virtual size: 3.9G (4194304000 bytes)
disk size: 3.9G
 
[root@localhost ~]# virsh shutdown centos6.5
域 centos6.5 被关闭
```

**格式转换，需要一些时间**

```
[root@localhost ~]# qemu-img convert -f raw -O qcow2 /data_kvm/store/KVM011.img /data_kvm/store/KVM011.qcow2
 
[root@localhost ~]# ls /data_kvm/store/
KVM011.img  KVM011.qcow2
 
[root@localhost ~]# ls /etc/libvirt/qemu
autostart  centos6.5_bak.xml  centos6.5.xml  networks
 
[root@localhost ~]# virsh edit centos6.5
编辑了域 centos6.5 XML 配置。
```

**修改centos6.5的xml配置文件**
 
```xml
23       <driver name='qemu' type='qcow2' cache='none'/>
24       <source file='/data_kvm/store/KVM011.qcow2'/>
```

## Virt-*工具的使用

### Virt-cat命令，类似于cat，可查看虚拟机里的文件
 
查看虚拟机里的network文件，需要些时间
```
[root@localhost ~]# virt-cat -a /data_kvm/store/centos6.5.qcow2 /etc/sysconfig/network
```

### Virt-edit命令，用于编辑文件，用法与vim基本相同
```
[root@localhost ~]# virt-edit -a /data_kvm/store/centos6.5.qcow2 /etc/sysconfig/networ
``` 
 
### Virt-df命令用查看虚拟机磁盘信息
```
[root@localhost ~]# virt-df -h centos6.5
Filesystem                                Size       Used  Available  Use%
centos6.5:/dev/sda1                       484M        33M       427M    7%
centos6.5:/dev/sdb1                       4.2G       4.2G          0  100%
centos6.5:/dev/VolGroup/lv_root           3.0G       1.0G       1.8G   34%
``` 
 
 
### 虚拟机的克隆

``` 
[root@localhost ~]# virsh destroy centos6.5
域 centos6.5 被删除
 
[root@localhost ~]# virsh list --all
 Id    名称                         状态
----------------------------------------------------
 -     centos6.5                      关闭
 ```

从centos6.5克隆为centos6.5-clome
```
[root@localhost ~]# virt-clone -o centos6.5 -n centos6.5-clome -f /data_kvm/store/KVM011-clone.qcow2
 
Clone 'centos6.5-clome' created successfully.
 
[root@localhost ~]# virsh list --all
 Id    名称                         状态
----------------------------------------------------
 -     centos6.5                      关闭
 -     centos6.5-clome                关闭
 ```

### 虚拟机的快照

**创建快照**
```
[root@localhost ~]# virsh snapshot-create centos6.5
Domain snapshot 1535644190 created
 
1535644190：快照的版本号（距离1970年1月1日过去了多少秒）
``` 

**查看快照信息**

```
[root@localhost ~]# virsh snapshot-list centos6.5
 名称               Creation Time             状态
------------------------------------------------------------
 1535644190           2018-08-30 23:49:50 +0800 shutoff
``` 
 
 
**恢复快照**
```
[root@localhost ~]# virsh snapshot-list centos6.5
 名称               Creation Time             状态
------------------------------------------------------------
 1535644190           2018-08-30 23:49:50 +0800 shutoff
 1535644574           2018-08-30 23:56:14 +0800 shutoff
 
[root@localhost ~]# virsh snapshot-revert centos6.5 1535644190
[root@localhost ~]# virsh snapshot-current centos6.5
<domainsnapshot>
  <name>1535644190</name>
  <state>shutoff</state>
```
 
**删除快照**
```
[root@localhost ~]# virsh snapshot-delete centos6.5 1535644190
Domain snapshot 1535644190 deleted
 
[root@localhost ~]# virsh snapshot-list centos6.5
 名称               Creation Time             状态
------------------------------------------------------------
 1535644574           2018-08-30 23:56:14 +0800 shutoff
 ```