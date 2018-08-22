---
title: "Call Opencv Used Python3 - 通过docker在python3.6下调用opencv3.4.1"
date: 2017-06-21T15:46:06+08:00
categories: ["Raspberry-Pi","All"]
tags: ["Raspberry-Pi", "Docker","Opencv","ARM","ARM64"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Raspberry-Pi", "Docker","Opencv"]
description: "通过docker在python3.6下调用opencv3.4.1"
---


## 根据之前build好的镜像，启动容器

启动一个带有python3.6 和 opencv3.4.1的环境

```
docker run -itd --name cv -v `pwd`/app:/app --net host -w /app jermine/opencv:alpine-arm64
```

## 进入容器

```
docker exec -it cv sh
```
