---
title: "Setup Docker for Pi - 树莓派安装docker"
date: 2017-07-11T16:05:54+08:00
categories: ["Raspberry-Pi","All"]
tags: ["Raspberry-Pi", "Docker","ARM","ARM64"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Raspberry-Pi", "Docker"]
description: "Setup Docker for Pi - 树莓派安装docker"
---

## 安装步骤

### 先下载对应的deb包：

```
wget https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/arm64/docker-ce_18.06.0~ce~3-0~ubuntu_arm64.deb 
```


### 通过dpkg进行安装 

直接 `dpkg -i *.deb` 进行安装


### 然后设置开机启动


```
systemctl enable docker
```

### 配置镜像加速：

```
cat > /etc/docker/daemon.json
{
    "registry-mirrors": [
        "https://2nmcv9vp.mirror.aliyuncs.com"
    ],
    "insecure-registries": []
}

```

重启服务

```
systemctl restart docker
```
