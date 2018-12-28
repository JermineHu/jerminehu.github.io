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

## 五、错误解决


### 错误 1 

```
apt-get -f install 错误 No apport report written because MaxReports is reached already Errors were...
```

安装cuda 时提示以下错误，按照要求执行了以后错误任然出现，执行`sudo apt-get update `和 `sudo apt-get upgrade` 以后依然是同样的错误。

```
sudo apt-get upgrade

Reading package lists... Done

Building dependency tree       
Reading state information... Done
You might want to run 'apt-get -f install' to correct these.
The following packages have unmet dependencies:
 cuda-curand-dev-8-0 : Depends: cuda-curand-8-0 (>= 8.0.64) but it is not installed
 cuda-cusolver-dev-8-0 : Depends: cuda-cusolver-8-0 (>= 8.0.64) but it is not installed
 cuda-cusparse-dev-8-0 : Depends: cuda-cusparse-8-0 (>= 8.0.64) but it is not installed
 cuda-nvgraph-dev-8-0 : Depends: cuda-nvgraph-8-0 (>= 8.0.64) but it is not installed
 cuda-nvrtc-dev-8-0 : Depends: cuda-nvrtc-8-0 (>= 8.0.64) but it is not installed
 cuda-toolkit-8-0 : Depends: cuda-nvml-dev-8-0 (>= 8.0.64) but it is not installed
E: Unmet dependencies. Try using -f.

```

在尝试了 

```
sudo apt-get clean
sudo apt-get update
sudo apt-get dist-upgrade
sudo dpkg --configure -a
sudo apt-get install -f
```
都还存在同样问题时需要手动打开status

sudo vi /var/lib/dpkg/status 

然后通过命令行输入 /name 比如 /cuda-curand-8-0  然后按Enter 键（下一个关键字按n）查找文件中的对应模块记录并将其删除（删除按dd），把以上所有模块记录全部删除后 shift+:组合，输入 wq！保存并退出，最后再次执行sudo apt-get install --f 。

### 错误 2

Ubuntu16.04 由于已经达到 MaxReports 限制，没有写入 apport 报告。

```
dpkg: 处理软件包 libzbar-dev:amd64 (--configure)时出错：
 依赖关系问题 - 仍未被配置
由于已经达到 MaxReports 限制，没有写入 apport 报告。

E: Sub-process /usr/bin/dpkg returned an error code (1)
```

从网上找到的解决方案为：

```
$ cd /var/lib/dpkg  
$ sudo mv info/ info-bak  
$ sudo mkdir info  
$ sudo apt-get update  
$ sudo apt-get install -f  

```
就解决了问题，然后恢复info-bak

```
$ sudo mv info/* info-bak/  
$ sudo rm -rf info  
$ sudo mv info-bak/ info  

```
一切就ok

### 错误3 

`Failed to initialize NVML: Driver/library version mismatch`
刚刚GPU遇到一个神奇的bug。

运行 `nvidia-smi` 报错：

`Failed to initialize NVML: Driver/library version mismatch`

运行nvidia 官方的程序，报错

 `no CUDA-capable device is detected`
如下图： 

![nvidia-app](/img/linux/5.png)
 

然后解决的办法是： 重启。。。重启。。。

还好虚惊一场，只能说：

 `Surprise surprise, rebooting solved the issue。`