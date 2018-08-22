---
title: "Make an Alpine Docker Image"
date: 2016-05-21T16:14:53+08:00
categories: ["Raspberry-Pi","All"]
tags: ["Raspberry-Pi", "Docker","ARM","ARM64"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Raspberry-Pi", "Docker"]
description: "Make an Alpine Docker Image - 在树莓派上构建一个arm64的docker镜像"
---

## 步骤：

### 运行如下代码获取基础镜像


```
docker pull jermine/alpine
```

Dockerfile 源码参考：https://github.com/JermineHu/docker-alpine-armhf

基于基础镜像可以安装alpine是所有软件，然后构建一个运行环境，比如

```
FROM jermine/alpine:arm64-3.7

RUN apk add golang ;\
    go env

```
