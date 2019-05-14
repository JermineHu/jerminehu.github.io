---
title: "Htop的安装和使用"
date: 2018-09-07T13:19:47+08:00
categories: ["All","Linux","efficiency"]
tags: ["linux","efficiency"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Linux","htop","efficiency" ]
description: "Htop的安装和使用"
---

### 一．Htop的使用简介

大家可能对top监控软件比较熟悉，今天我为大家介绍另外一个监控软件Htop，姑且称之为top的增强版，相比top其有着很多自身的优势。如下：

* 两者相比起来，top比较繁琐
* 默认支持图形界面的鼠标操作
* 可以横向或纵向滚动浏览进程列表，以便看到所有的进程和完整的命令行
* 杀进程时不需要输入进程号等

### 二．软件的获取与安装

Htop的安装，既可以通过源码包编译安装，也可以配置好yum源后网络下载安装

#### 源码安装

在htop的项目官方网站上：http://sourceforge.net/projects/htop/ 直接下载即可

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210952_604.png)

由于我已经安装过了，因此大家看个以上每个编译过程后面都会^c，是不执行此行操作Ctrl+c取消的，此处只是告知如何编译安装的，各人的环境不同，可能编译过程中会出现错误，根据错误，解决后即可。

#### yum和rpm包安装

个人推荐yum安装，能够自动的解决软件包依赖关系，安装即可。

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210952_198.png)

### 三．Htop的使用

安装完成后，命令行中直接敲击htop命令，即可进入htop的界面

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210952_64.png)

各项从上至下分别说明如下：

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210953_803.png)

左边部分从上至下，分别为，cpu、内存、交换分区的使用情况，右边部分为：Tasks为进程总数，当前运行的进程数、Load average为系统1分钟，5分钟，10分钟的平均负载情况、Uptime为系统运行的时间。

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210953_984.png)

以上各项分别为：

* PID：进行的标识号
* USER：运行此进程的用户
* PRI：进程的优先级
* NI：进程的优先级别值，默认的为0，可以进行调整
* VIRT：进程占用的虚拟内存值
* RES：进程占用的物理内存值
* SHR：进程占用的共享内存值
* S：进程的运行状况，R表示正在运行、S表示休眠，等待唤醒、Z表示僵死状态
* %CPU：该进程占用的CPU使用率
* %MEM：该进程占用的物理内存和总内存的百分比
* TIME+：该进程启动后占用的总的CPU时间
* COMMAND：进程启动的启动命令名称
  
 ![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210953_292.png)


Shortcut Key | Function Key | Description | 中文说明
-------------|--------------|-------------|-----
h, ? | F1 | Invoke htop Help | 查看htop使用说明
S | F2 | Htop Setup Menu | htop 设定
/ | F3 | Search for a Process | 搜索进程
\ | F4 | Incremental process filtering | 增量进程过滤器
t | F5 | Tree View | 显示树形结构
<, > | F6 | Sort by a column | 选择排序方式
[ | F7 | Nice - (change priority) | 可减少nice值，这样就可以提高对应进程的优先级
] | F8 | Nice + (change priority) | 可增加nice值，这样就可以降低对应进程的优先级
k | F9 | Kill a Process | 可对进程传递信号
q | F10 | Quit htop | 结束htop


#### 命令行选项（COMMAND-LINE OPTIONS）

参数 | 说明
--------------|---------------------------------------
-C --no-color |　 使用一个单色的配色方案
-d --delay=DELAY | 设置延迟更新时间，单位秒
-h --help | 显示htop 命令帮助信息
-u --user=USERNAME | 只显示一个给定的用户的过程
 -p --pid=PID,PID…　 | 只显示给定的PIDs
-s --sort-key COLUMN| 依此列来排序
-v –version　　　　　　　 | 显示版本信息



#### 交互式命令（INTERACTIVE COMMANDS）

上下键或`PgUP, PgDn` 选定想要的进程，左右键或`Home, End `移动字段，当然也可以直接用鼠标选定进程；

字符 | 说明
------- | -------
Space | 标记/取消标记一个进程。命令可以作用于多个进程，例如 "kill"，将应用于所有已标记的进程
U | 取消标记所有进程
s | 选择某一进程，按s:用strace追踪进程的系统调用
l | 显示进程打开的文件: 如果安装了lsof，按此键可以显示进程所打开的文件
I | 倒转排序顺序，如果排序是正序的，则反转成倒序的，反之亦然
+, - | When in tree view mode, expand or collapse subtree. When a subtree is collapsed a "+" sign shows to the left of the process name.
a (在有多处理器的机器上) | 设置 CPU affinity: 标记一个进程允许使用哪些CPU
u | 显示特定用户进程
M | 按Memory 使用排序
P | 按CPU 使用排序
T | 按Time+ 使用排序
F | 跟踪进程: 如果排序顺序引起选定的进程在列表上到处移动，让选定条跟随该进程。这对监视一个进程非常有用：通过这种方式，你可以让一个进程在屏幕上一直可见。使用方向键会停止该功能。
K | 显示/隐藏内核线程
H | 显示/隐藏用户线程
Ctrl-L | 刷新
Numbers | PID 查找: 输入PID，光标将移动到相应的进程上


**F1：显示帮助信息**

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210954_995.png)

**F2 Htop设定**

鼠标点击Setup或者按下F2 之后进入htop 设定的页面，

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210954_435.png)

**Setup 选项下的：**

1. Meters

设定顶端的 显示信息，分为左右两侧，Left column 表示左侧的显示的信息，Right column表示右侧显示的信息，如果要新加选项，可以选择Available meters添加，F5新增到上方左侧，F6新增到上方右侧。Left column和Right column下面的选项，可以选定信息的显示方式，有LED、Bar(进度条)、Text(文本模式)，可以根据个人喜好进行设置

2. Display options

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210955_617.png)

选择要显示的内容，按空格 x表示显示，选择完后，按F10保存

3.Colors

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210955_510.png)

设定界面以什么颜色来显示，个人认为用处不大，各人喜好不同，假如我们选择Black on White后显示效果如下

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210955_77.png)

4.Colums

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210955_60.png)

作用是增加或取消要显示的各项内容，选择后F7(向上移动)、F8(向下移动)、F9(取消显示、F10(保存更改))此处增加了PPID、PGRP，根据各人需求，显示那些信息。

**F3 搜索进程**

在界面下按F3或直接输入”/”就可以直接进入搜索模式，是按照进程名进行搜索的。例如

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210956_380.png)

搜索到的进程会用设定的颜色标记出来，方便查看

**F4：过滤器**

相当于模糊查找，不区分大小写，下方输入要搜索的内容后，则界面只显示搜索到的内容，更加方便查看，例如：

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210956_321.png)

**F5:以树形方式显示**

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210956_876.png)

**F6：排序方式**

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210956_8.png)

按下F6后会跳转至上图界面，让您选择以什么方式进行排序,在Sort by下选择您要以什么来排序

**F7,F8：调整进程nice值**

F7表示减小nice值(增大优先级),F8增大nice值(减小优先级)，选择某一进程，按F7或F8来增大或减小nice值，nice值范围为-20-19，此处我把apache的nice值调整到了19

![Linux htop工具使用详解](https://static.open-open.com/lib/uploadImg/20141203/20141203210957_254.png)

**F9：杀死进程**

选择某一进程按F9即可杀死此进程，很方便

**F10:退出htop**