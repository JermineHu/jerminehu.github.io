---
title: "K8s on Raspberry Pi"
date: 2017-10-21T16:20:55+08:00
categories: ["Raspberry-Pi","All"]
tags: ["Raspberry-Pi", "Docker","ARM","ARM64","Kubernetes","k8s"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 80
keywords: ["Raspberry-Pi", "Docker","ARM","ARM64","Kubernetes","k8s"]
description: "K8s on Raspberry Pi - 树莓派搭建k8s节点"
---


## 配置环境步骤如下：

### 安装kubeadm必要的软件

```
apt install socat ebtables ethtool

```

### 安装相关的软件

主要软件有：

**kubeadm_1.10.2-00_arm64 、kubectl_1.10.2-00_arm64 、kubelet_1.10.2-00_arm64 、kubernetes-cni_0.6.0-00_arm64**

下载地址为：https://mirrors.ustc.edu.cn/kubernetes/apt/pool/

将上述arm64的deb包安装完即可通过kubeadm创建集群或者加入集群