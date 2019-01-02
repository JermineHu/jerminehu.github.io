---
title: "ubuntu 16.04 [re]install CUDA - Ubuntu 安装 CUDA"
date: 2018-08-24T13:48:25+08:00
categories: ["All","Linux","CUDA",conda,python]
tags: ["Linux","CUDA",conda,python]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Linux","CUDA",conda,python]
description: "ubuntu 16.04 [re]install CUDA - Ubuntu 安装 CUDA"
---

## 推荐玩法：

**注意** ： 由于tensorflow的GPU版本依赖nvidia的cuda、cudnn库，因此一般需要包含cuda和cudnn的链接库文件，普遍做法是通过主机安装cudnn、cuda的方式。这里还有另外两种方式可以选择：

### 方法一：使用Miniconda

采用miniconda安装GPU版本的tensorflow会自动安装依赖的cuda、cudnn动态库以及其他python库，因此使用conda在安装tensorflow的时候可以灵活选择tensorflow版本和cuda版本，装完就能直接跑。

####　安装方法：

```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && bash Miniconda3-latest-Linux-x86_64.sh

```

#### 离线使用步骤：

1、在有网的机器上通过conda把环境装好（主要就是tensorflow、numpy等）；

2、然后把miniconda文件夹打个压缩包；

3、cp到U盘；

4、从U盘cp到没有网的机器上；

5、配置miniconda的环境变量（export PATH=$PATH:/usr/local/miniconda3/bin）就好了；


### 方法二：使用nvidia-docker

 由于程序运行于容器之中，所以镜像中一般都是带有CUDA、CUDNN库的，因此只需要在docker所在的主机上安装显卡驱动即可，无需费太大力气去安装cuda、cuddn之类的东西。 参照： https://jermine.vdo.pub/linux/linux%E5%AE%89%E8%A3%85nvidia-docker/ 进行安装

### 总结：

直接运行在主机的项目，采用方法一更加方便；运行于docker中的项目更推荐采用nvidia-docker。

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


如果需要安装其他版本可以打开 https://developer.nvidia.com/cuda-90-download-archive （cuda-90改成你需要的版本）然后下载，然后离线安装

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

