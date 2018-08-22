+++
date = "2016-03-20T12:13:19+08:00"
title = "在docker中搭建Samba服务器"
categories = ["Docker","All"]
tags = ["Docker", "Samba"]
toc = true
author = "Jermine"
author_homepage =  "/"
weight= 70
keywords= ["Docker", "Samba"]
description= "在docker中搭建Samba服务器"
+++

## 在docker中搭建Samba服务器


要是想把容器的权限与宿主主机的用户权限一致的话，则只需要把用户和组文件映射到容器里面即可：


```
docker run --restart=always -d --name samba -p 139:139 -p 445:445 -v /data/samba_server:/share -v /etc/passwd:/etc/passwd:ro -v /etc/group:/etc/group:ro dperson/samba -s "lnh;/share/;yes;no;no;all;none"
```

如果只是设置账户直接访问可以直接run：

```
docker run --restart=always -d --name samba --net host -v /data/samba_server:/share  dperson/samba -u "samba;password" -s "samba;/share/;yes;no;no;all;none"
```
