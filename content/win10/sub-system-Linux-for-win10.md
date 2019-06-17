---
title: "Sub System Linux for Win10 - Win10设置Linux子系统"
date: 2017-09-07T10:29:47+08:00
categories: ["All","Linux",windows]
tags: ["windows", "Linux"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 50
keywords: ["Win10", "Linux"]
description: "Sub System Linux for Win10 - Win10设置Linux子系统"
---

按如下步骤进行安装：

# 设置win10开启Linux

![linux子系统](/img/win10/1836534-05d8e85c3ba54a60.png)

![linux子系统](/img/win10/1836534-780f6423bd5160f2.png)

![linux子系统](/img/win10/1836534-05bbfeca9c2f7aae.png)


系统升级到一周年正式版及以上(1607)

依次在 设置 - 更新与安全 - 针对开发人员 选项中，启用"开发人员模式"

在资源管理器中打开 控制面板\所有控制面板项\程序和功能 , 打开 启用或关闭 Windows功能 , 勾选 适用于Linux的Windows子系统(Beta)

重启电脑

命令行运行 lxrun /install /y 开始安装
安装速度取决于网络情况，下载的文件在 %localappdata%\lxss 目录下 lxss.tar.gz (181M)，解压后大概500M, rootfs 目录即为子系统根目录。

命令行运行 bash 进入Ubuntu
默认使用的 root 帐号登录，通过指令 passwd 设置密码。

注：本文脚本均在root帐号下操作，因此建议使用root帐号
毕竟爱折腾，难免会把子系统环境(lxss目录)玩坏掉，因此干正事前最好先备份下以便快速还原。注意，不要直接右键复制或者打包，可能会导致文件权限丢失的。
xcopy %localappdata%\lxss %localappdata%\lxss.bak /E
当然，如果你比较任性也可以不执行上一步的备份操作，通过命令行运行 lxrun /uninstall /full 轻松卸载子系统，重复上面的步骤即可重装，不过要注意下载速度时好时坏哦。
通过上面的步骤，已经启用了win10自带的linux子系统(WSL)，感觉逼格提升了不少。当然，怎么能满足于此呢，接下来就要做一些环境的配置和进一步的挖掘。


# 安装Win10的Linux子系统

1. 系统升级到一周年正式版及以上(1607)

1. 依次在 `设置 - 更新与安全 - 针对开发人员` 选项中，启用"开发人员模式"

1. 在资源管理器中打开 `控制面板\所有控制面板项\程序和功能` , 打开 `启用或关闭 Windows功能` , 勾选 `适用于Linux的Windows子系统(Beta)`

1. 重启电脑

1. 命令行运行 `lxrun /install /y `开始安装
 安装速度取决于网络情况，下载的文件在 `%localappdata%\lxss` 目录下 `lxss.tar.gz` (181M)，解压后大概500M, `rootfs` 目录即为子系统根目录。

1. 命令行运行 `bash` 进入`Ubuntu`
 默认使用的 `root` 帐号登录，通过指令 `passwd` 设置密码。

  **注：本文脚本均在root帐号下操作，因此建议使用root帐号**

7. 毕竟爱折腾，难免会把子系统环境(lxss目录)玩坏掉，因此干正事前最好先备份下以便快速还原。注意，不要直接右键复制或者打包，可能会导致文件权限丢失的。
`xcopy %localappdata%\lxss %localappdata%\lxss.bak /E`

8. 当然，如果你比较任性也可以不执行上一步的备份操作，通过命令行运行 `lxrun /uninstall /full `轻松卸载子系统，重复上面的步骤即可重装，不过要注意下载速度时好时坏哦。


通过上面的步骤，已经启用了win10自带的linux子系统(WSL)，感觉逼格提升了不少。当然，怎么能满足于此呢，接下来就要做一些环境的配置和进一步的挖掘。


# 使用win10中的Linux子系统

## 1、更换源

```
sudo sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
```


## 2、更新

```
sudo apt-get update
```


## 3、安装桌面和工具


```
sudo apt-get -y install zsh xfce4 vnc4server
```


## 4、修改配置文件

```
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
sudo vim /etc/ssh/sshd_config
#=======(修改以下选项内容)=========#
# Port 23（22端口已被占用）        #
# (取消注释)ListenAddress 0.0.0.0 #
# UsePrivilegeSeparation no      #
# PermitRootLogin yes            #
# （注释）StrictModes yes         #
# PasswordAuthentication yes     #
#================================#
```

## 3、启动ssh

```
sudo service ssh start
```

## 4、如果提示“sshd error: could not load host key”，则用下面的命令重新生成

```
rm /etc/ssh/ssh*key
dpkg-reconfigure openssh-server
```


## 5、配置环境变量

```
echo "export DISPLAY=:0.0" >> ~/.bashrc && source ~/.bashrc
```


## 6、中文支持


```
sudo apt-get install language-pack-zh-hant language-pack-zh-hans

sudo vi /etc/environment
```


## 增加如下内容

```
LANG="zh_CN.UTF-8"
LANGUAGE="zh_CN:zh:en_US:en"
```


## 然后再安装支持的语言

```
sudo apt install $(check-language-support)
```


## 重新配置本地化，使其生效

```
sudo dpkg-reconfigure locales
```


## 7、输入法

```
apt-get install ibus ibus-clutter ibus-gtk ibus-gtk3 ibus-qt4
im-config -s ibus ibus-pinyin
```


## 8、VNC 使用

```
# 修改xstartup, 将 x-window-manager 替换为刚才安装的 xfce4-session
sed -i 's$x-window-manager$xfce4-session$' ~/.vnc/xstartup
# 重启 vncserver
vncserver -kill :1
vncserver :1

#分辨率设置
# 先关闭
vncserver -kill :1
# 再启动并设置分辨率(注意是小写的x)
vncserver -geometry 1080x1900 :1
```


 ==注意：==

安装完之后以管理员身份运行刚刚安装的Ubuntu应用，然后就是进行Ubuntu系统初始化操作，提示你输入一个用户名和密码等，按照提示输入好即可，很简单

参照网址：

https://www.cnblogs.com/wurui1994/p/7839777.html

https://www.jianshu.com/p/bc38ed12da1d

安装目录地址：

C:\Users\Husee\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu18.04onWindows_79rhkp1fndgsc\LocalState\rootfs

