---
title: "Set Mirrors for Pi - 树莓派配置加速镜像"
date: 2017-08-21T16:09:28+08:00
categories: ["Raspberry-Pi","All"]
tags: ["Raspberry-Pi", "Mirrors"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 20
keywords: ["Raspberry-Pi", "Mirrors","Ubuntu"]
description: "Set Mirrors for Pi - 树莓派配置加速镜像"
---


## 执行如下命令完成配置
```
root@raspi:/home/pi# cat > /etc/apt/sources.list
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi
#deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi

```

然后执行

```
sudo apt update
```