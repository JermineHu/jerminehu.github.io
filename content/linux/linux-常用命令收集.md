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

**根据大小倒叙排列当前目录信息**

```
du -sm $(ls -A) | sort -nr
```

**获取指定目录下每个目录或文件的大小**

```
du -sh $(find /home  -maxdepth 1 -depth)
```

**获取指定目录下每个目录或文件的大小并倒叙排列**

```
du -sm $(find /home  -maxdepth 1 -depth) | sort -nr
```

## 查看目录下有多少文件

```
ls -l |grep "^-"|wc -l
```

或

```
find ./company -type f | wc -l
```

查看某文件夹下文件的个数，包括子文件夹里的。

```
ls -lR|grep "^-"|wc -l
```

查看某文件夹下文件夹的个数，包括子文件夹里的。

```
ls -lR|grep "^d"|wc -l
```

说明：

```
ls -l
```

长列表输出该目录下文件信息(注意这里的文件，不同于一般的文件，可能是目录、链接、设备文件等)

```
grep "^-"
```
这里将长列表输出信息过滤一部分，只保留一般文件，如果只保留目录就是 ^d

```
wc -l
```

统计输出信息的行数，因为已经过滤得只剩一般文件了，所以统计结果就是一般文件信息的行数，又由于

一行信息对应一个文件，所以也就是文件的个数。


查看目录下有多少个文件及文件夹需在终端输入

```
ls | wc -w
```

查看目录下有多少个文件需在终端输入

```
ls | wc -c
```

查看文件夹下有多少个文件，多少个子目录需在终端输入

```
ls -l |wc -l
```
若只想知道文件的个数，则需在终端输入

```
/bin/ls -l |grep ^-|wc -l
```