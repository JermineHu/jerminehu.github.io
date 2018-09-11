---
title: "Linux 常用命令收集"
date: 2018-09-11T09:43:59+08:00
categories: ["All","Linux"]
tags: ["linux",]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["linux","常用命令" ]
description: "Linux 常用命令收集"
---

## 查找指定目录下符合条件的文件

```
ls -lh $(find /home -type f -size +100M) #查找指定目录下文件的大小

ls -lh $(find /home -type f -size +10M -a -size -100M）# 查找大于10M且小于100M的文件

find / -name "*.mp3" -type f -size +100M |xargs rm -rf #删除单个文件超过100M的MP3文件

```

## 获取指定目录信息

**获取指定目录的绝对地址**
```
find /home  -maxdepth 1
```

**获取指定目录的列表信息**
```
find /home  -maxdepth 1 | xargs ls -lh 分组打印各个目录下的信息
find /home  -maxdepth 1 | xargs ls -ld #只打印指定目录的信息
```
**通过sed获取绝对地址**
```
ls | sed "s:^:`pwd`/:" # 在每行记录的开头加上当前路径

```

## 查看目录大小

**获取当前目录下每个目录或文件的大小**
```
du -sh  $(ls)
或者
du -sh * 
```

**获取指定目录下每个目录或文件的大小**

```
du -sh * $(find /home  -maxdepth 1)
```

