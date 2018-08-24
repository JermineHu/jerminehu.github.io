---
title: "ubuntu 16.04 [re]install CUDA - Ubuntu 安装 CUDA"
date: 2018-08-24T13:48:25+08:00
categories: ["All","Linux","CUDA"]
tags: ["Linux","CUDA"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Linux","CUDA"]
description: "ubuntu 16.04 [re]install CUDA - Ubuntu 安装 CUDA"
---

## 一、卸载原有cuda

参考：https://blog.csdn.net/weixin_40294256/article/details/79173174

```
$ sudo apt-get remove cuda
$ sudo apt-get autoclean
$ sudo apt-get remove cuda*
$ cd /usr/local
$ sudo rm -rf cuda-9.1

```

卸载完成后，建议重启一下。

## 二、安装

### step1. 停掉所有与nvidia或者cuda相关进程

参考：https://github.com/NVIDIA/nvidia-docker/issues/62

```
$ sudo service lightdm stop
$ sudo stop nvidia-digits-server
$ sudo service docker stop
$ sudo rmmod nvidia-uvm
$  sudo service nvidia-docker stop
```

### step2. 下载 .run 安装脚本

下载地址：https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=runfilelocal

### step3. 验证 gcc g++ 版本为 5.4.0

### step4. 执行 run 脚本

```
$ sudo sh cuda_9.1.85_387.26_linux.run
```
执行过程中，有很多对话框选择，有默认值的选择默认值，没有默认值的选择yes

### step5. 恢复安装前停掉的服务

或者

### 另一种方式（也是最简单的）：

采用网络安装方式：
https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=debnetwork


1. `wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_9.2.148-1_amd64.deb`
2. `sudo dpkg -i cuda-repo-ubuntu1604_9.2.148-1_amd64.deb`
3. `sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub`
4. `sudo apt-get update`
5. `sudo apt-get install cuda`

## 三、测试cuda

```
$ cd /usr/local/cuda/samples/1_Utilities/deviceQuery 
$ sudo make
$ ./deviceQuery
```

如果
`error: CUDA driver version is insufficient for CUDA runtime version Result=FAIL`
则说明没有安装成功。

## 四、运行`nvidia-docker`

参考：https://github.com/nvidia/nvidia-container-runtime#docker-engine-setup
