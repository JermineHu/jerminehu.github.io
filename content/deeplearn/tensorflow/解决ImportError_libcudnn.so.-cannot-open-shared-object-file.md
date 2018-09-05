---
title: "解决TensorFlow：ImportError: libcudnn.so.*: cannot open shared object file: No such file or dictionary"
date: 2018-09-05T15:18:21+08:00
categories: ["All","DeepLearn","Tensorflow","cuda"]
tags: ["DeepLearn","Tensorflow","cuda"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["DeepLearn","Tensorflow" ,"cuda"]
description: "解决TensorFlow：ImportError: libcudnn.so.*: cannot open shared object file: No such file or dictionary"
---

### 环境

```
Ubuntu16.04
Nvidia 384
CUDA 8.0
cuDNN 5

```

### 错误

TensorFlow、CUDA、cuDNN的版本关系我时常懵懵的，经常出现各种不支持的情况，比如今天安装了TensorFlow1.10.1，报错:`ImportError: libcudnn.so.7: cannot open shared object file: No such file or dictionary`

### 错误缘由
google了一圈发现， 问题出在 `TensorFlow 1.10.1-gpu` 是基于`cuDNN7`，需要的也就是`libcudnn.so.7`了。

### 解决方案：

Just download cuDNN v6 and follow the steps (Tested on Ubuntu 16.04, CUDA toolkit 9.0 ) https://developer.nvidia.com/cudnn 


<h4>Download cuDNN v7.2.1 (August 7, 2018), for CUDA 9.2</h4>
<div class="panel-body">
	
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.2_20180806/cudnn-9.2-linux-x64-v7.2.1.38">cuDNN v7.2.1 Library for Linux</a></p>
			<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.2_20180806/cudnn-9.2-linux-ppc64le-v7.2.1.38">cuDNN v7.2.1 Library for Linux (Power8/Power9)</a></p>

<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.2_20180806/cudnn-9.2-windows7-x64-v7.2.1.38">cuDNN v7.2.1 Library for Windows 7</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.2_20180806/cudnn-9.2-windows10-x64-v7.2.1.38">cuDNN v7.2.1 Library for Windows 10</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.2_20180806/cudnn-9.2-osx-x64-v7.2.1.38">cuDNN v7.2.1 Library for OSX</a></p>

<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.2_20180806/Ubuntu16_04-x64/libcudnn7_7.2.1.38-1_cuda9.2_amd64">cuDNN v7.2.1 Runtime Library for Ubuntu16.04  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.2_20180806/Ubuntu16_04-x64/libcudnn7-dev_7.2.1.38-1_cuda9.2_amd64&#10;">cuDNN v7.2.1 Developer Library for Ubuntu16.04  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.2_20180806/Ubuntu16_04-x64/libcudnn7-doc_7.2.1.38-1_cuda9.2_amd64">cuDNN v7.2.1 Code Samples and User Guide for Ubuntu16.04  (Deb)</a></p>

<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.2_20180806/Ubuntu16_04-ppc64le/libcudnn7_7.2.1.38-1_cuda9.2_ppc64el">cuDNN v7.2.1 Runtime Library for Ubuntu16.04 &amp; Power8  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.2_20180806/Ubuntu16_04-ppc64le/libcudnn7-dev_7.2.1.38-1_cuda9.2_ppc64el">cuDNN v7.2.1 Developer Library for Ubuntu16.04 &amp; Power8  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.2_20180806/Ubuntu16_04-ppc64le/libcudnn7-doc_7.2.1.38-1_cuda9.2_ppc64el">cuDNN v7.2.1 Code Samples and User Guide for Ubuntu16.04 &amp; Power8 (Deb)</a></p>

<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.2_20180806/Ubuntu14_04-x64/libcudnn7_7.2.1.38-1_cuda9.2_amd64&#10;">cuDNN v7.2.1 Runtime Library for Ubuntu14.04  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.2_20180806/Ubuntu14_04-x64/libcudnn7-dev_7.2.1.38-1_cuda9.2_amd64&#10;">cuDNN v7.2.1 Developer Library for Ubuntu14.04  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.2_20180806/Ubuntu14_04-x64/libcudnn7-doc_7.2.1.38-1_cuda9.2_amd64&#10;">cuDNN v7.2.1 Code Samples and User Guide for Ubuntu14.04  (Deb)</a></p>
	</div>

<h4>Download cuDNN v7.2.1 (August 7, 2018), for CUDA 9.0</h4>
<div class="panel-body">
		<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.0_20180806/cudnn-9.0-linux-x64-v7.2.1.38">cuDNN v7.2.1 Library for Linux</a></p>
			<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.0_20180806/cudnn-9.0-linux-ppc64le-v7.2.1.38">cuDNN v7.2.1 Library for Linux (Power8)</a></p>

<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.0_20180806/cudnn-9.0-windows7-x64-v7.2.1.38">cuDNN v7.2.1 Library for Windows 7</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.0_20180806/cudnn-9.0-windows10-x64-v7.2.1.38">cuDNN v7.2.1 Library for Windows 10</a></p>

<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.0_20180806/Ubuntu16_04-x64/libcudnn7_7.2.1.38-1_cuda9.0_amd64">cuDNN v7.2.1 Runtime Library for Ubuntu16.04  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.0_20180806/Ubuntu16_04-x64/libcudnn7-dev_7.2.1.38-1_cuda9.0_amd64&#10;">cuDNN v7.2.1 Developer Library for Ubuntu16.04  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.0_20180806/Ubuntu16_04-x64/libcudnn7-doc_7.2.1.38-1_cuda9.0_amd64">cuDNN v7.2.1 Code Samples and User Guide for Ubuntu16.04  (Deb)</a></p>

<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.0_20180806/Ubuntu16_04-ppc64le/libcudnn7_7.2.1.38-1_cuda9.0_ppc64el">cuDNN v7.2.1 Runtime Library for Ubuntu16.04 &amp; Power8  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.0_20180806/Ubuntu16_04-ppc64le/libcudnn7-dev_7.2.1.38-1_cuda9.0_ppc64el">cuDNN v7.2.1Developer Library for Ubuntu16.04 &amp; Power8  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.0_20180806/Ubuntu16_04-ppc64le/libcudnn7-doc_7.2.1.38-1_cuda9.0_ppc64el">cuDNN v7.2.1 Code Samples and User Guide for Ubuntu16.04 &amp; Power8 (Deb)</a></p>

<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.0_20180806/Ubuntu14_04-x64/libcudnn7_7.2.1.38-1_cuda9.0_amd64&#10;">cuDNN v7.2.1 Runtime Library for Ubuntu14.04  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.0_20180806/Ubuntu14_04-x64/libcudnn7-dev_7.2.1.38-1_cuda9.0_amd64&#10;">cuDNN v7.2.1 Developer Library for Ubuntu14.04  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/9.0_20180806/Ubuntu14_04-x64/libcudnn7-doc_7.2.1.38-1_cuda9.0_amd64&#10;">cuDNN v7.2.1 Code Samples and User Guide for Ubuntu14.04  (Deb)</a></p>
	</div>

  
<h4>  Download cuDNN v7.2.1 (August 7, 2018), for CUDA 8.0</h4>

<div class="panel-body">

<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/8.0_20180806/cudnn-8.0-linux-x64-v7.2.1.38">cuDNN v7.2.1 Library for Linux</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/8.0_20180806/cudnn-8.0-windows7-x64-v7.2.1.38">cuDNN v7.2.1 Library for Windows 7</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/8.0_20180806/cudnn-8.0-windows10-x64-v7.2.1.38">cuDNN v7.2.1 Library for Windows 10</a></p>

<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/8.0_20180806/Ubuntu16_04-x64/libcudnn7_7.2.1.38-1_cuda8.0_amd64">cuDNN v7.2.1 Runtime Library for Ubuntu16.04  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/8.0_20180806/Ubuntu16_04-x64/libcudnn7-dev_7.2.1.38-1_cuda8.0_amd64&#10;">cuDNN v7.2.1 Developer Library for Ubuntu16.04  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/8.0_20180806/Ubuntu16_04-x64/libcudnn7-doc_7.2.1.38-1_cuda8.0_amd64">cuDNN v7.2.1 Code Samples and User Guide for Ubuntu16.04  (Deb)</a></p>

<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/8.0_20180806/Ubuntu14_04-x64/libcudnn7_7.2.1.38-1_cuda8.0_amd64&#10;">cuDNN v7.2.1 Runtime Library for Ubuntu14.04  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/8.0_20180806/Ubuntu14_04-x64/libcudnn7-dev_7.2.1.38-1_cuda8.0_amd64">cuDNN v7.2.1 Developer Library for Ubuntu14.04  (Deb)</a></p>
<p><a target="_blank" href="https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.2.1/prod/8.0_20180806/Ubuntu14_04-x64/libcudnn7-doc_7.2.1.38-1_cuda8.0_amd64">cuDNN v7.2.1 Code Samples and User Guide for Ubuntu14.04  (Deb)</a></p>
	</div>


```
$ tar xvzf cudnn-8.0-linux-x64-v5.1-ga.tgz 
$ sudo cp -P cuda/include/cudnn.h /usr/local/cuda/include 
$ sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64 
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*

```
Now set Path variables

```
$ vim ~/.bashrc 
```

翻到最底部加上： 

```
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64" 
export CUDA_HOME=/usr/local/cuda
```
