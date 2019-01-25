---
title: "基于Docker部署ceph分布式文件系统[MImic13.2"
date: 2019-01-24T15:32:11+08:00
categories: ["All",ceph,linux,docker]
tags: [ceph,linux,docker]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: [ceph,linux,docker]
description: "基于Docker部署ceph分布式文件系统[MImic13.2"
---

> 本文记录了基于docker部署最新ceph版本的多节点高可用测试详尽过程，切身体会比ceph-deploy方便得多，希望能给初次接触ceph的同学提供些许参考。

## 一、环境准备

| host名称 | IP地址 | 备注 |
| ------ | ------ | ------ |
| node-1 | 192.168.16.197 | mon、osd、mgr、rgw、mds、rbd |
| node-2 | 192.168.16.148 | mon、osd、mgr、rgw、mds、rbd |
| node-3 | 192.168.16.222 | osd |
| client(本地) | 192.168.16.221 | ceph客户端 |

### 1.配置hosts

##### 在三台服务器上初始化：
```
cat >> /etc/hosts <<EOF
192.168.16.197 ceph1
192.168.16.148 ceph2
192.168.16.222 ceph3
EOF
```

### 2.建立信任关系

```
#在192.168.1.197（ceph1）上执行：
ssh-keygen
#把密钥发给ceph2、ceph3
ssh-copy-id ceph2 
ssh-copy-id ceph3
```

### 3.磁盘初始化

**在没有物理磁盘或者没有物理分区情况下，可以通以下方式进行实验。**

初始化两个10g的镜像文件。

```
dd if=/dev/zero of=ceph-disk-01 bs=1G count=10
dd if=/dev/zero of=ceph-disk-02 bs=1G count=10
```

使用 losetup将磁盘镜像文件虚拟成快设备

```
sudo losetup -f ceph-disk-01
sudo losetup -f ceph-disk-02
```

格式化

```
sudo mkfs.ext4 -f /dev/loop15 #名称根据fdisk -l进行查询
sudo mkfs.ext4 -f /dev/loop16
```

挂载文件系统

```
sudo mount /dev/loop15 ceph01/
sudo mount /dev/loop16 ceph02/
```

### 4.安装docker
**可自行搞定，也可以如下使用 yum 安装：**

```
yum install -y docker
systemctl enable docker
systemctl start docker
```

### 5.拉取ceph
这里用到了 dockerhub 上最流行的 ceph/daemon 镜像

```
docker pull ceph/daemon:latest
```

### 6.创建目录
```
mkdir ~/ceph/{admin,etc,lib,logs}
chown -R 167:167 ~/ceph/  #docker内用户id是167，这里进行授权
```

### 7.内核优化
```
#调整内核参数
cat >> /etc/sysctl.conf << EOF
kernel.pid_max=4194303
vm.swappiness = 0
EOF
sysctl -p
 
# read_ahead, 通过数据预读并且记载到随机访问内存方式提高磁盘读操作，根据一些Ceph的公开分享，8192是比较理想的值
echo "8192" > /sys/block/sda/queue/read_ahead_kb
 
# I/O Scheduler，关于I/O Scheculder的调整，简单说SSD要用noop，SATA/SAS使用deadline。
echo "deadline" > /sys/block/sda/queue/scheduler
echo "noop" > /sys/block/sda/queue/scheduler
```

### 8.其他配置

把容器内的 ceph 命令 alias 到本地，方便使用，其他 ceph 相关命令也可以参考添加：

```
echo 'alias ceph="docker exec mon ceph"' >> /etc/profile
source /etc/profile
```

## 二、部署ceph

> **以下操作均在 192.168.16.197(ceph1)上进行。**

### 1.编写脚本
1.monitor

```
vim ~/ceph/admin/start_mon.sh
```

```
#!/bin/bash
docker run -d --net=host \
    --name=mon \
    --restart=always \
    -v /etc/localtime:/etc/localtime \
    -v ~/ceph/etc:/etc/ceph \
    -v ~/ceph/lib:/var/lib/ceph \
    -v ~/ceph/logs:/var/log/ceph \
    -e MON_IP=192.168.16.197,192.168.16.148,192.168.16.222 \
    -e CEPH_PUBLIC_NETWORK=192.168.16.0/24 \
    ceph/daemon:latest mon
```
> 注：Monitor 节点这里有个很关键的配置：MON_IP 和 CEPH_PUBLIC_NETWORK 要写全，比如本文有 3 台服务器，那么 MAN_IP 需要写上 3 个 IP，而且如果 IP 跨网段，那么 CEPH_PUBLIC_NETWORK 必须写上所有网段，因为这个脚本在后面会发送到其他服务器执行！

2.OSD（Object Storage Device）

```
vim ~/ceph/admin/start_osd.sh
```
```
#!/bin/bash
# 这里表示有11个分区，及从data1-data11，请根据实际情况修改：
for i in {1..2};do
    docker ps | grep -w osd_data${i} && continue
    docker ps -a | grep -w osd_data${i} && docker rm -f osd_data${i}
 
    docker run -d \
    --name=osd_data${i} \
    --net=host \
    --restart=always \
    --privileged=true \
    --pid=host \
    -v /etc/localtime:/etc/localtime \
    -v ~/ceph/etc:/etc/ceph \
    -v ~/ceph/lib:/var/lib/ceph \
    -v ~/ceph/logs:/var/log/ceph \
    -v ~/ceph/data/data${i}/osd:/var/lib/ceph/osd \
    ceph/daemon:latest osd_directory
done
```
3.MDS（Metadata Server）

```
vim ~/ceph/admin/start_mds.sh
```

```
#!/bin/bash
docker run -d \
    --net=host \
    --name=mds \
    --restart=always \
    --privileged=true \
    -v /etc/localtime:/etc/localtime \
    -v ~/ceph/etc:/etc/ceph \
    -v ~/ceph/lib/:/var/lib/ceph/ \
    -v ~/ceph/logs/:/var/log/ceph/ \
    -e CEPHFS_CREATE=0 \
    -e CEPHFS_METADATA_POOL_PG=512 \
    -e CEPHFS_DATA_POOL_PG=512 \
    ceph/daemon:latest mds
```

4.MGR（Manager Daemon）

```
vim ~/ceph/admin/start_mgr.sh
```
```
#!/bin/bash
docker run -d --net=host \
    --name=mgr \
    --restart=always \
    --privileged=true \
    -v /etc/localtime:/etc/localtime \
    -v ~/ceph/etc:/etc/ceph \
    -v ~/ceph/lib:/var/lib/ceph \
    -v ~/ceph/logs:/var/log/ceph \
    ceph/daemon:latest mgr
```

5.RGW（RADOS Gateway）

```
vim ~/ceph/admin/start_rgw.sh
```
```
#!/bin/bash
docker run -d \
    --net=host \
    --name=rgw \
    --restart=always \
    --privileged=true \
    -v /etc/localtime:/etc/localtime \
    -v ~/ceph/etc:/etc/ceph \
    -v ~/ceph/lib:/var/lib/ceph \
    -v ~/ceph/logs:/var/log/ceph \
    ceph/daemon:latest rgw
```
6.RBD（Rados Block Device）

```
vim ~/ceph/admin/start_rbd.sh
```
```
#!/bin/bash
docker run -d \
    --net=host \
    --name=rbd \
    --restart=always \
    --privileged=true \
    -v /etc/localtime:/etc/localtime \
    -v ~/ceph/etc:/etc/ceph \
    -v ~/ceph/lib:/var/lib/ceph \
    -v ~/ceph/logs:/var/log/ceph \
    ceph/daemon:latest rbd_mirror
```

### 2.启动mon
1.在192.168.16.197（ceph1）上运行start_mon.sh，成功启动后会生成配置数据。在ceph主配置文件中，追加如下内容：

```
cat >>/home/yscz/ceph/etc/ceph.conf <<EOF
# 容忍更多的时钟误差
mon clock drift allowed = 2
mon clock drift warn backoff = 30
# 允许删除pool
mon_allow_pool_delete = true
 
[mgr]
# 开启WEB仪表盘
mgr modules = dashboard
EOF
```

2.拷贝所有数据（已包含脚本）到另外2台服务器

```
scp -r ~/ceph ceph2:/home/yscz/
scp -r ~/ceph ceph3:/home/yscz/
```

3.通过远程ssh，在ceph2和ceph3上依次启动mon

```
ssh ceph2 bash ~/ceph/admin/start_mon.sh
ssh ceph3 bash ~/ceph/admin/start_mon.sh
```

4.查看集群状态

```
ceph -s
```
>如果能够看到 ceph2 和 ceph3 则表示集群创建成功，此时应该是 HEALTH_WARN 状态，因为还没有创建 osd。

### 2.启动OSD
虽然 ceph/daemon 这个 docker 镜像支持一个镜像来启动多个 osd，映射到多块分区，但是为了方便管理，需要为每一块磁盘创建一个 osd。

在 3 台服务器上依次执行 start_osd.sh 脚本：

```
bash ~/ceph/admin/start_osd.sh
ssh ceph2 bash ~/ceph/admin/start_osd.sh
ssh ceph3 bash ~/ceph/admin/start_osd.sh
```

全部 osd 都启动后，稍等片刻后执行 ceph -s 查看状态，应该可以看到多了如下信息（总共6块盘，6个OSD）：

```
osd: 6 osds: 6 up, 6 in
```

### 3.启动MDS

```
bash ~/ceph/admin/start_osd.sh
ssh ceph2 bash ~/ceph/admin/start_osd.sh
```

### 4.创建文件系统

```
#创建data pool
ceph osd pool create cephfs_data 128 128

#创建metadata pool
ceph osd pool create cephfs_metadata 128 128

#创建cephfs
ceph fs new cephfs cephfs_metadata cephfs_data

#查看信息
ceph fs ls

name: cephfs, metadata pool: cephfs_metadata, data pools: [cephfs_data ]
```

### 5.启动其他组件
1.启动RGW

```
bash ~/ceph/admin/start_rgw.sh
ssh ceph1 bash ~/ceph/admin/start_rgw.sh
```

2.启动RBD

```
bash ~/ceph/admin/start_rbd.sh
ssh ceph1 bash ~/ceph/admin/start_rbd.sh
ssh ceph2 bash ~/ceph/admin/start_rbd.sh
```

### 6.启动MGR

MGR 是 ceph 自带的仪表盘监控，可以在一台服务器上单点启动也可以多点启动实现多活：

```
bash /home/yscz/ceph/admin/start_mgr.sh
ssh ceph2 bash /home/yscz/ceph/admin/start_mgr.sh
```

启动后，在 ceph1 节点上执行如下命令激活仪表盘：

```
ceph mgr module enable dashboard
ceph config-key put mgr/dashboard/server_addr 0.0.0.0
ceph config-key put mgr/dashboard/server_port 7000 #指定为7000端口，这里可以自定义修改
```
>启动成功后，通过浏览器访问 http://192.168.1.100:7000 即可看到仪表盘。

### 7.最终状态

```
docker ps -a
```
```
CONTAINER ID        IMAGE                COMMAND                  CREATED             STATUS              PORTS               NAMES
449f8c4c0f65        ceph/daemon:latest   "/opt/ceph-container…"   26 hours ago        Up 26 hours                             osd_data2
28f90768ec8b        ceph/daemon:latest   "/opt/ceph-container…"   26 hours ago        Up 26 hours                             osd_data1
14580b7ecb14        ceph/daemon:latest   "/opt/ceph-container…"   28 hours ago        Up 28 hours                             rgw
46f70c9e019b        ceph/daemon:latest   "/opt/ceph-container…"   28 hours ago        Up 21 hours                             mgr
6820235c582e        ceph/daemon:latest   "/opt/ceph-container…"   28 hours ago        Up 28 hours                             mds
24918de9292a        ceph/daemon:latest   "/opt/ceph-container…"   28 hours ago        Up 28 hours                             mon
```


```
ceph -s
```
```
cluster:
    id:     37f6f306-7c90-493a-82d9-17888daddf21
    health: HEALTH_OK
 
  services:
    mon: 2 daemons, quorum y-99-03,ceph01
    mgr: ceph01(active), standbys: y-99-03
    mds: cephfs-1/1/1 up  {0=ceph01=up:active}, 1 up:standby
    osd: 6 osds: 6 up, 6 in
    rgw: 2 daemons active
 
  data:
    pools:   6 pools, 288 pgs
    objects: 241  objects, 3.4 KiB
    usage:   1.3 GiB used, 39 GiB / 40 GiB avail
    pgs:     288 active+clean
```

	
## 附：参考文档：

https://hub.docker.com/r/ceph/daemon
https://www.jianshu.com/p/f08ed7287416
https://www.cnblogs.com/dowi/p/9633150.html
https://zhangge.net/5136.html