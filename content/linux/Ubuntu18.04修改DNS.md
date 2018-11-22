---
title: "Ubuntu18.04修改DNS"
date: 2018-11-22T14:56:29+08:00
categories: ["All","Linux"]
tags: ["Linux","DNS"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Linux","DNS","Ubuntu18.04" ]
description: "Ubuntu18.04修改DNS"
---

## Ubuntu18.04修改DNS

### 问题描述

最近使用了最新版的`ubuntu 18.04`运行一些服务，然后发现服务器经常出现网络不通的情况，主要是一些域名无法解析。

检查`/etc/resolv.conf`，发现之前修改的nameserver总是会被修改为`127.0.0.53`，无论是改成啥，过段时间，总会变回来。

### 问题排查

查看`/etc/resolv.conf`这个文件的注释，发现开头就写着这么一行：

`# This file is managed by man:systemd-resolved(8). Do not edit.`
这说明这个文件是被systemd-resolved这个服务托管的。

通过`netstat -tnpl| grep systemd-resolved`查看到这个服务是监听在53号端口上。

查了下，这个服务的配置文件为`/etc/systemd/resolved.conf`，大致内容如下：

```
[Resolve]
DNS=1.1.1.1 1.0.0.1
#FallbackDNS=
#Domains=
LLMNR=no
#MulticastDNS=no
#DNSSEC=no
#Cache=yes
#DNSStubListener=yes
```

### 问题解决

如果我们要想让/etc/resolve.conf文件里的配置生效，需要添加到systemd-resolved的这个配置文件里DNS配置项（如上面的示例，已经完成修改），然后`sudo systemctl restart systemd-resolved.service`重启systemd-resolved服务即可。

另一种更简单的办法是，我们直接停掉systemd-resolved服务，这样再修改/etc/resolve.conf就可以一直生效了。