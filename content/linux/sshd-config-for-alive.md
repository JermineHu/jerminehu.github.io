---
title: "SSH Config for Alive - 解决ssh经常断开的问题"
date: 2016-05-12T09:54:32+08:00
categories: ["All","Linux"]
tags: ["SSH", "Linux"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 50
keywords: ["SSH", "Linux","sshd"]
description: "SSH Config for Alive - 解决ssh经常断开的问题"
---

SSH连接总是隔一段时间没有输入时就断开，解决办法如下：

### 服务端配置

```
sudo vi /etc/ssh/sshd_config
ClientAliveInterval 60     #服务端主动向客户端请求响应的间隔
ClientAliveCountMax 10    #服务器发出请求后客户端没有响应的次数达到一定值就自动断开
sudo restart ssh
```

### 客户端配置 
sudo vi /etc/ssh/ssh_config  #或~/.ssh/config



```
ConnectTimeout 15
ConnectionAttempts 3
ServerAliveInterval 20
ServerAliveCountMax 5
TCPKeepAlive=yes
ServerAliveInterval 60 #客户端主动向服务端请求响应的间隔
```

或


```
ssh -i <key-file> -o StrictHostKeyChecking=no -o TCPKeepAlive=yes -o ServerAliveInterval=30 ubuntu@<ip>
```

其原理是，本地 ssh 客户端每隔 30s 向服务器端 sshd 发送 keep-alive 数据包，如果连续发送 10 次，server 无回应，则断开连接。这样同时规避了网络闪断的问题。

虽然可以在服务器端设置 ClientAliveInterval 来实现同样的效果，在客户端做更合适。

上面方式任选一种，我选客户端配置方式。
