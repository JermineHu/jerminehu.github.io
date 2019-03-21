---
title: "Linux保存依赖安装包到指定目录(用于离线安装)"
date: 2017-03-21T16:00:00+08:00
categories: ["All",Linux]
tags: ["Linux",]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Linux",“yum”,aptitude ]
description: "Linux保存依赖安装包到指定目录"
---

经常与一些保密级别较高的企业打交道，他们使用的网络都是内部网络，根本无法访问到公网，但是部署一些项目时难免会有依赖包需要离线安装。遇到这种问题通常都是手动去下载依赖的库，然后离线安装，但是你无法判断下载的这个依赖库是否还依赖其他库，如果是手动去下载安装将耗费很长时间（他们导入任何文件都要走流程，一次就需要4h左右），为了改善这种情况，就需要将所有依赖都离线下载到本，本文将介绍如何将Centos和Ubuntu的依赖包保存到本地。

### CentOS下载 RPM 包及其所有依赖包（推荐yumdownloader）

#### 方法一：

我们可以通过 `yum `命令的 `Downloadonly` 插件下载 RPM 软件包及其所有依赖包。
为了安装 `Downloadonly `插件，以 `root` 身份运行以下命令。

```
yum install yum-plugin-downloadonly
```

现在，运行以下命令去下载一个 RPM 软件包。

```
yum install --downloadonly <package-name>
```
默认情况下，这个命令将会下载并把软件包保存到 `/var/cache/yum/` 的 `rhel-{arch}-channel/packageslocation` 目录，不过，你也可以下载和保存软件包到任何位置，你可以通过 `–downloaddir` 选项来指定。

```
yum install --downloadonly --downloaddir=<directory> <package-name>
```

例子:

```
yum install --downloadonly --downloaddir=/root/mypackages/ httpd
```

正如你在上面输出所看到的, httpd软件包已经被依据所有依赖性下载完成了 。
请注意，这个插件适用于` yum install/yum update`， 但是并不适用于 `yum groupinstall` 。默认情况下，这个插件将会下载仓库中最新可用的软件包。然而你可以通过指定版本号来下载某个特定的软件版本。
例子:

```
yum install --downloadonly --downloaddir=/root/mypackages/ httpd-2.2.6-40.el7
```
此外，你也可以如下一次性下载多个包：

```
yum install --downloadonly --downloaddir=/root/mypackages/ httpd vsftpd
```

#### 方法二：

Yumdownloader是一款简单，但是却十分有用的命令行工具，它可以一次性下载任何 RPM 软件包及其所有依赖包。
以 root 身份运行如下命令安装 Yumdownloader 工具。

```
yum install yum-utils
```
一旦安装完成，运行如下命令去下载一个软件包，例如 httpd。

```
yumdownloader httpd
```

为了根据所有依赖性下载软件包，我们使用 --resolve参数：

```
yumdownloader --resolve httpd
```
默认情况下，Yumdownloader 将会下载软件包到当前工作目录下。
为了将软件下载到一个特定的目录下，我们使用 --destdir 参数：

```
yumdownloader --resolve --destdir=/root/mypackages/ httpd
```
或者，
```
yumdownloader --resolve --destdir /root/mypackages/ httpd
```
终端输出：

```
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: centos.excellmedia.net
 * epel: epel.mirror.angkasa.id
 * extras: centos.excellmedia.net
 * updates: centos.excellmedia.net
--> Running transaction check
---> Package httpd.x86_64 0:2.4.6-40.el7.centos.4 will be installed
--> Processing Dependency: httpd-tools = 2.4.6-40.el7.centos.4 for package: httpd-2.4.6-40.el7.centos.4.x86_64
--> Processing Dependency: /etc/mime.types for package: httpd-2.4.6-40.el7.centos.4.x86_64
--> Processing Dependency: libaprutil-1.so.0()(64bit) for package: httpd-2.4.6-40.el7.centos.4.x86_64
--> Processing Dependency: libapr-1.so.0()(64bit) for package: httpd-2.4.6-40.el7.centos.4.x86_64
--> Running transaction check
---> Package apr.x86_64 0:1.4.8-3.el7 will be installed
---> Package apr-util.x86_64 0:1.5.2-6.el7 will be installed
---> Package httpd-tools.x86_64 0:2.4.6-40.el7.centos.4 will be installed
---> Package mailcap.noarch 0:2.1.41-2.el7 will be installed
--> Finished Dependency Resolution
(1/5): apr-util-1.5.2-6.el7.x86_64.rpm | 92 kB 00:00:01 
(2/5): mailcap-2.1.41-2.el7.noarch.rpm | 31 kB 00:00:02 
(3/5): apr-1.4.8-3.el7.x86_64.rpm | 103 kB 00:00:02 
(4/5): httpd-tools-2.4.6-40.el7.centos.4.x86_64.rpm | 83 kB 00:00:03 
(5/5): httpd-2.4.6-40.el7.centos.4.x86_64.rpm | 2.7 MB 00:00:19
```

让我们确认一下软件包是否被下载到我们指定的目录下。

```
ls /root/mypackages/
```

终端输出：
```
apr-1.4.8-3.el7.x86_64.rpm
apr-util-1.5.2-6.el7.x86_64.rpm
httpd-2.4.6-40.el7.centos.4.x86_64.rpm
httpd-tools-2.4.6-40.el7.centos.4.x86_64.rpm
mailcap-2.1.41-2.el7.noarch.rpm
```

不像 Downloadonly 插件，Yumdownload 可以下载一组相关的软件包。 `yumdownloader "@Development Tools" --resolve --destdir /root/mypackages/`
我更推荐使用Yumdownloader ，但是两者都是十分简单易懂而且可以完成相同的工作。

### Ubuntu 下载 deb 包及其所有依赖包

```
# aptitude clean
# aptitude --download-only install <your_package_here>
# cp /var/cache/apt/archives/*.deb <your_directory_here>

```

或者

```
sudo apt-get install --reinstall -d $(apt-cache depends kubeadm | grep Depends |cut -d: -f2 | tr -d "<>")

```


```
#!/bin/bash

logfile=/home/perrin/Desktop/log
ret=""
function getDepends()
{
   echo "fileName is" $1>>$logfile
   # use tr to del < >
   ret=`apt-cache depends $1|grep Depends |cut -d: -f2 |tr -d "<>"`
   echo $ret|tee  -a $logfile
}
# 需要获取其所依赖包的包
libs="gnome-shell"                  # 或者用$1，从命令行输入库名字

# download libs dependen. deep in 3
i=0
while [ $i -lt 3 ] ;
do
    let i++
    echo $i
    # download libs
    newlist=" "
    for j in $libs
    do
        added="$(getDepends $j)"
        newlist="$newlist $added"
        apt install $added --reinstall -d -y
    done

    libs=$newlist
done

```

参照: https://blog.csdn.net/junbujianwpl/article/details/52811153