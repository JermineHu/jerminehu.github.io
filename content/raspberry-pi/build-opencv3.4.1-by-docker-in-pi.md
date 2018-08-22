---
title: "Build Opencv3.4 by docker in Raspberry pi - 在树莓派上通过docker编译opencv3.4.1"
date: 2018-06-21T15:31:01+08:00
categories: ["Raspberry-Pi","All"]
tags: ["Raspberry-Pi", "Docker","Opencv","ARM","ARM64"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Raspberry-Pi", "Docker","Opencv"]
description: "Build Opencv3.4 by docker in Raspberry pi;在树莓派上通过docker编译opencv3.4.1"
---


### 采用docker编译如下：

为了能在docker执行的时候加速，采用了`--build-arg`参数，设定了代理地址

#### 在X86上编译如下：

docker build  --build-arg HTTP_PROXY=http://192.168.16.254:1080 --build-arg HTTPS_PROXY=http://192.168.16.254:1080 -t jermine/opencv:alpine -f Dockerfile.alpine .


#### 在ARM64上编译操作如下
docker build -t jermine/opencv:alpine-arm64-3.4.1 -f Dockerfile.alpine --build-arg HTTP_PROXY=http://192.168.16.254:1080 --build-arg HTTPS_PROXY=http://192.168.16.254:1080 .


### 环境变量配置如下：
```
export OPENCV_DIR=/opt/opencv
export LIBGPUARRAY_DIR=/opt/libgpuarray
export NUM_CORES=8
export NB_UID=1000
export CLONE_TAG=1.0
export OPENCV_VERSION=3.4.1
export OPENCL_ENABLED=OFF
```

### 需要的库如下：
```
apt install g++ python3 python3-dev build-essential cmake pkg-config libprotobuf-dev protobuf-compiler libopencv-dev


wget https://bootstrap.pypa.io/get-pip.py 

python3 get-pip.py

```

#### Cmake的配置如下：

```
    
    -D BUILD_opencv_xfeatures2d=OFF \
    -D BUILD_opencv_world=OFF \

    cmake \
    -D PYTHON_EXECUTABLE=$(which python3) \
    -D WITH_CUDA=OFF \
    -D CMAKE_BUILD_TYPE=RELEASE \
    -D BUILD_PYTHON_SUPPORT=ON \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_C_EXAMPLES=ON \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D BUILD_PYTHON_SUPPORT=ON \
    -D BUILD_NEW_PYTHON_SUPPORT=ON \
    -D PYTHON_DEFAULT_EXECUTABLE=$(which python3) \
    -D PYTHON_INCLUDE_DIR=/usr/include/python3.5m \
    -D PYTHON_LIBRARY=/usr/lib/arm-linux-gnueabihf/libpython3.5m.so \
    -D PYTHON3_NUMPY_INCLUDE_DIRS=/usr/lib/python3/dist-packages/numpy/core/include \
    -D OPENCV_EXTRA_MODULES_PATH=$OPENCV_DIR/opencv_contrib-$OPENCV_VERSION/modules \
    -D WITH_TBB=ON \
    -D WITH_PTHREADS_PF=ON \
    -D WITH_OPENNI=OFF \
    -D WITH_OPENNI2=ON \
    -D WITH_EIGEN=ON \
    -D BUILD_DOCS=ON \
    -D BUILD_TESTS=ON \
    -D BUILD_PERF_TESTS=ON \
    -D BUILD_EXAMPLES=ON \
    -D WITH_OPENCL=$OPENCL_ENABLED \
    -D USE_GStreamer=ON \
    -D WITH_GDAL=ON \
    -D WITH_CSTRIPES=ON \
    -D ENABLE_FAST_MATH=1 \
    -D WITH_OPENGL=ON \
    -D WITH_QT=OFF \
    -D WITH_IPP=ON \
    -D WITH_FFMPEG=ON \
    -D WITH_V4L=ON .. && \
    make -j4 && \
    make install && \
    ldconfig && \
    
    
```

#### 具体参照我的Github源码

地址为：
https://github.com/JermineHu/docker-opencv