---
title: "解决树莓派交换空间的问题"
date: 2016-08-21T15:19:10+08:00
categories: ["Raspberry-Pi","All"]
tags: ["Raspberry-Pi", "Swap","ARM","ARM64"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Raspberry-Pi", "Swap"]
description: "解决树莓派交换空间的问题"
---


今天在树莓派编译一个较大的C项目时发现每次编译到43之后速度会特别慢并且死机，百度之后发现可能是交换空间不足，增加1G交换空间后才解决死机问题。

树莓派3B默认的swap空间为99m,这对于编译一些大点的项目显然有点不够看，很容易就会死机，以下给出增加swap的解决方案

在/opt/image中添加一块swap交换空间


```
cd /opt  
sudo mkdir image  
cd image  
sudo touch swap    #创建文件  
sudo dd if=/dev/zero of=/opt/image/swap bs=1024 count=1024000  #添加交换文件并设置为1G  
#过段时间会返回(这个略慢)  
#1024000+0 records in  
#1024000+0 records out  
#大小 copied, 所用时间 s, 速度 MB/s  
sudo mkswap /opt/image/swap  #设置交换空间  
  
sudo swapon /opt/image/swap  #启用新增的交换空间  
free -m    #确认是否已经生效
```

之后修改/etc/fstab文件使重启后这块swap也能生效,在文件最后添加

```
/opt/image/swap    /swap    swap    defaults 0 0
```
