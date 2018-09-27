---
title: "如何为Debian系统打deb包"
date: 2018-09-27T18:23:38+08:00
categories: ["All","Linux"]
tags: ["Linux",]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Linux","debian","ubuntu","deb","打包" ]
description: "如何为Debian系统打deb包"
---

## 源码编译安装

以dropwatch为例子，将其打成deb包。源码地址 ：https://github.com/pavel-odintsov/drop_watch

操作如下：

```
sudo apt-get install -y libnl-3-dev libnl-genl-3-dev binutils-dev libreadline6-dev
mkdir -pv ~/deb-{file,root}
cd ~/deb-file
git clone https://github.com/pavel-odintsov/drop_watch.git dropwatch
cd ~/deb-file/dropwatch/src
make
sudo cp -fv ~/deb-file/dropwatch/src/dropwatch /usr/bin/dropwatch
```

## 使用 fpm 快速打包

```
mkdir -pv ~/deb-root/dropwatch_1.4-1/usr/bin
cp -fv ~/deb-file/dropwatch/src/dropwatch ~/deb-root/dropwatch_1.4-1/usr/bin/dropwatch 
fpm -f -s dir -t deb -n dropwatch --epoch 1 -v 1.4 --iteration 1 -C ~/deb-root/dropwatch_1.4-1 -p ~/deb-file -d 'libnl-3-200' -d 'libnl-genl-3-200' --verbose --category 'Applications/System' --description 'Kernel dropped packet monitor' --url 'github.com/pavel-odintsov/drop_watch' --license 'GPLv2+' -m 'Jermine' usr/bin/dropwatch
```

打包成功输出信息如下：

```
Setting workdir {:workdir=>"/tmp", :level=>:info}
Setting from flags: category=Applications/System {:level=>:info}
Setting from flags: description=Kernel dropped packet monitor {:level=>:info}
Setting from flags: epoch=1 {:level=>:info}
Setting from flags: iteration=1 {:level=>:info}
Setting from flags: license=GPLv2+ {:level=>:info}
Setting from flags: maintainer=Jermine {:level=>:info}
Setting from flags: name=dropwatch {:level=>:info}
Setting from flags: url=fedorahosted.org/dropwatch {:level=>:info}
Setting from flags: version=1.4 {:level=>:info}
Converting dir to deb {:level=>:info}
epoch in Version is set {:epoch=>"1", :level=>:warn}
No deb_installed_size set, calculating now. {:level=>:info}
Reading template {:path=>"/var/lib/gems/2.1.0/gems/fpm-1.4.0/templates/deb.erb", :level=>:info}
Debian packaging tools generally labels all files in /etc as config files, as mandated by policy, so fpm defaults to this behavior for deb packages. You can disable this default behavior with --deb-no-default-config-files flag {:level=>:warn}
Creating {:path=>"/tmp/package-deb-build20160108-3103-169fb7d/control.tar.gz", :from=>"/tmp/package-deb-build20160108-3103-169fb7d/control", :level=>:info}
Creating boilerplate changelog file {:level=>:info}
Reading template {:path=>"/var/lib/gems/2.1.0/gems/fpm-1.4.0/templates/deb/changelog.erb", :level=>:info}
Created package {:path=>"~/deb-file/dropwatch_1.4-1_amd64.deb"}
```

## 使用 lesspipe 查看打包信息

```
lesspipe ~/deb-file/dropwatch_1.4-1_amd64.deb
~/deb-file/dropwatch_1.4-1_amd64.deb:
 new debian package, version 2.0.
 size 15896 bytes: control archive=420 bytes.
     306 bytes,    12 lines      control              
      52 bytes,     1 lines      md5sums              
 Package: dropwatch
 Version: 1:1.4-1
 License: GPLv2+
 Vendor: Jermine@compiler
 Architecture: amd64
 Maintainer: Jermine
 Installed-Size: 34
 Depends: libnl-3-200, libnl-genl-3-200
 Section: Applications/System
 Priority: extra
 Homepage: fedorahosted.org/dropwatch
 Description: Kernel dropped packet monitor
*** Contents:
drwx------ 0/0               0 2016-01-08 22:33 ./
drwxr-xr-x 0/0               0 2016-01-08 22:33 ./usr/
drwxr-xr-x 0/0               0 2016-01-08 22:33 ./usr/bin/
-rwxr-xr-x 0/0           35728 2016-01-08 22:33 ./usr/bin/dropwatch
drwxr-xr-x 0/0               0 2016-01-08 22:33 ./usr/share/
drwxr-xr-x 0/0               0 2016-01-08 22:33 ./usr/share/doc/
drwxr-xr-x 0/0               0 2016-01-08 22:33 ./usr/share/doc/dropwatch/
-rw-r--r-- 0/0             131 2016-01-08 22:33 ./usr/share/doc/dropwatch/changelog.Debian.gz
```

## dropwatch 使用

* 查看支持的模块列表 `dropwatch -l list`
* 监视指定模块 `dropwatch -l kas`
* 输入 `start` 开始
* 按 `Ctrl + c` 中止
* 输入`exit` 退出
  
```
Jermine@compiler:~$ dropwatch -l list
Available lookup methods:
kas - use /proc/kallsyms
Jermine@compiler:~$ dropwatch -l kas
Initalizing kallsyms db
dropwatch> start
Enabling monitoring...
Kernel monitoring activated.
Issue Ctrl-C to stop monitoring
1 drops at tcp_rcv_state_process+1b6 (0xffffffff8146ca66)
1 drops at sk_stream_kill_queues+4b (0xffffffff814130cb)
1 drops at tcp_rcv_state_process+1b6 (0xffffffff8146ca66)
1 drops at sk_stream_kill_queues+4b (0xffffffff814130cb)
2 drops at tcp_rcv_state_process+1b6 (0xffffffff8146ca66)
2 drops at sk_stream_kill_queues+4b (0xffffffff814130cb)
1 drops at ip_rcv_finish+16b (0xffffffff814534fb)
1 drops at tcp_rcv_state_process+1b6 (0xffffffff8146ca66)
1 drops at sk_stream_kill_queues+4b (0xffffffff814130cb)
1 drops at tcp_rcv_state_process+1b6 (0xffffffff8146ca66)
1 drops at sk_stream_kill_queues+4b (0xffffffff814130cb)
^CGot a stop message
dropwatch> exit
Shutting down ...
```

## 将包上传到仓库供其他机器使用

四步实现deb文件的分发和安装

1. 将本地打好的deb包上传到存储服务器的仓库中（Nexus、静态服务器等都可）；
2. 将仓库地址加入到客户机的`/etc/apt/source.list`文件中；
3. 执行`apt update`
4. 安装操作 `sudo apt-get -y install dropwatch`

最总结果如下图：

![image4](/img/linux/4.png)
