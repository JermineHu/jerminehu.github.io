---
title: "Git设置和取消代理"
date: 2017-03-30T10:16:38+08:00
categories: ["All","Git","代理"]
tags: ["Git","代理"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Git","代理" ]
description: "Git设置和取消代理"
---

## git 设置和取消代理

本地开启VPN后，GIt也需要设置代理，才能正常略过GFW，访问goole code等网站

### 代理设置如下（可复制）：

注：git 设置 socks5 代理 加速。只对http，https生效，对ssh仍然无效

```
git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy https://127.0.0.1:1080
git config --global http.proxy 'socks5://127.0.0.1:1080' 
git config --global https.proxy 'socks5://127.0.0.1:1080'
```


### 取消代理设置如下

```
git config --global --unset http.proxy
git config --global --unset https.proxy
```
