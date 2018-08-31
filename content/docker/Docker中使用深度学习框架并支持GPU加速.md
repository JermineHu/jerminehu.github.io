---
title: "Docker中使用深度学习框架并支持GPU加速"
date: 2017-08-23T15:40:16+08:00
categories: ["All","Linux","DeepLearn","docker"]
tags: ["Linux","DeepLearn","docker","tensorflow"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Linux","DeepLearn","docker","tensorflow"]
description: "Docker中使用深度学习框架并支持GPU加速"
---

## Docker中使用深度学习框架并支持GPU加速

### 启动一个支持gpu的容器

```
docker run --runtime=nvidia --restart=always --name tensorflow  -dit -v `pwd`:/app -w /app  nvidia/cuda:9.0-cudnn7-runtime-ubuntu16.04
```
### 进入容器

```
docker exec -it tensorflow bash

```

### 设置源


```

Jermine@ubuntu:~$ cat > /etc/apt/sources.list

deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
##测试版源
deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse

```
### 更新源

```
sudo apt-get update 
```

### 安装相关依赖


```
# 导入环境变量
 TENSORFLOW_VERSION=1.7.0 
 
 apt-get update -y  && apt-get install -y --no-install-recommends python3 python3-pip  protobuf-compiler;\
    pip3 install --upgrade pip ;\
    python3 -V && pip3 -V ;\
    pip3  --no-cache-dir install setuptools  ;\
    pip3 --no-cache-dir install  \
    https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-${TENSORFLOW_VERSION}-cp35-cp35m-linux_x86_64.whl ;\
    apt-get autoremove && apt-get autoclean ;\
    rm -rf /var/lib/apt/lists/*
```

### 测试程序


```
import numpy as np
np.random.seed(0)
import tensorflow as tf
import time

N,D = 6000,8000

with tf.device('/cpu:0'):
    x = tf.placeholder(tf.float32)
    y = tf.placeholder(tf.float32)
    z = tf.placeholder(tf.float32)

    a = x * y
    b = a + z
    c = tf.reduce_sum(b)

grad_x, grad_y, grad_z = tf.gradients(c, [x,y,z])

start_time = time.time()
with tf.Session() as sess:
    values = {
        x: np.random.randn(N, D),
        y: np.random.randn(N, D),
        z: np.random.randn(N, D),
    }
    out = sess.run([c, grad_x, grad_y, grad_z],
                   feed_dict=values)
    c_val, grad_x_val, grad_y_val, grad_z_val = out
elapsed = time.time() - start_time
print(time.strftime("%H:%M:%S", time.gmtime(elapsed)))
print("exit 0")

```
将其存为 test_gpu_for_tensorflow.py  ， 使用 `python3 test_gpu_for_tensorflow.py ` 执行结果如下：


```
root@3c87c7061bf7:/app# python3 test_gpu.py 
2018-04-13 07:52:15.770002: I tensorflow/core/platform/cpu_feature_guard.cc:140] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX2 FMA
2018-04-13 07:52:17.657962: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1344] Found device 0 with properties: 
name: TITAN X (Pascal) major: 6 minor: 1 memoryClockRate(GHz): 1.531
pciBusID: 0000:02:00.0
totalMemory: 11.91GiB freeMemory: 11.75GiB
2018-04-13 07:52:17.919721: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1344] Found device 1 with properties: 
name: TITAN X (Pascal) major: 6 minor: 1 memoryClockRate(GHz): 1.531
pciBusID: 0000:82:00.0
totalMemory: 11.91GiB freeMemory: 11.75GiB
2018-04-13 07:52:17.926481: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1423] Adding visible gpu devices: 0, 1
2018-04-13 07:52:18.632238: I tensorflow/core/common_runtime/gpu/gpu_device.cc:911] Device interconnect StreamExecutor with strength 1 edge matrix:
2018-04-13 07:52:18.632315: I tensorflow/core/common_runtime/gpu/gpu_device.cc:917]      0 1 
2018-04-13 07:52:18.632331: I tensorflow/core/common_runtime/gpu/gpu_device.cc:930] 0:   N N 
2018-04-13 07:52:18.632342: I tensorflow/core/common_runtime/gpu/gpu_device.cc:930] 1:   N N 
2018-04-13 07:52:18.632981: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1041] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:0 with 11381 MB memory) -> physical GPU (device: 0, name: TITAN X (Pascal), pci bus id: 0000:02:00.0, compute capability: 6.1)
2018-04-13 07:52:18.893349: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1041] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:1 with 11381 MB memory) -> physical GPU (device: 1, name: TITAN X (Pascal), pci bus id: 0000:82:00.0, compute capability: 6.1)
00:00:11
exit 0

```


==如果遇到如下问题可以采用==`apt update && apt install libgomp1 `

```
ImportError: libgomp.so.1: cannot open shared object file: No such file or directory

```


