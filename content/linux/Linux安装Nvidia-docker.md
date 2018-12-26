---
title: "Linux安装Nvidia Docker"
date: 2018-12-26T18:14:00+08:00
categories: ["All","Linux",Nvidia,"Docker"]
tags: ["Linux",Nvidia,"Docker"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Linux",Nvidia,"Docker" ]
description: "Linux安装Nvidia Docker"
---

**注意：** 由于程序运行于容器之中，所以镜像中一般都是带有CUDA、CUDNN库的，因此只需要在docker所在的主机上安装显卡驱动即可，无需费太大力气去安装cuda、cuddn之类的东西。

让docker支持显卡调用分以下几个步骤：

## 安装docker 

通过官方源安装docker可以参照 https://docs.docker.com/install/linux/docker-ce/ubuntu/

**或者直接下载安装：**

```
wget https://download.docker.com/linux/ubuntu/dists/xenial/pool/stable/amd64/docker-ce_18.09.0~3-0~ubuntu-xenial_amd64.deb

dpkg -i docker-ce_18.09.0~3-0~ubuntu-xenial_amd64.deb

```

## 安装显卡驱动

先关闭图形化，可以参照： https://jermine.vdo.pub/linux/%E5%90%AF%E7%94%A8%E6%88%96%E5%85%B3%E9%97%AD%E5%9B%BE%E5%BD%A2%E5%8C%96/

然后进行显卡安装，操作如下：

```
wget http://cn.download.nvidia.com/XFree86/Linux-x86_64/410.78/NVIDIA-Linux-x86_64-410.78.run

chmod +x NVIDIA-Linux-x86_64-410.78.run

sudo ./NVIDIA-Linux-x86_64-410.78.run

```

## 安装Nvidia-docker

官方文档：https://github.com/NVIDIA/nvidia-docker


```
# If you have nvidia-docker 1.0 installed: we need to remove it and all existing GPU containers
docker volume ls -q -f driver=nvidia-docker | xargs -r -I{} -n1 docker ps -q -a -f volume={} | xargs -r docker rm -f
sudo apt-get purge -y nvidia-docker

# Add the package repositories
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update

# Install nvidia-docker2 and reload the Docker daemon configuration
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd

# Test nvidia-smi with the latest official CUDA image
docker run --runtime=nvidia --rm nvidia/cuda:9.0-base nvidia-smi
```

