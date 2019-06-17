---
title: "Openshift Setup"
date: 2019-03-15T09:12:34+08:00
categories: ["All",kubernetes,openshift]
tags: [kubernates,openshift]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: [kubernates,openshift]
description: "Openshift Setup"
---

# 介绍：

OpenShift是红帽的云开发平台即服务（PaaS）。自由和开放源码的云计算平台使开发人员能够创建、测试和运行他们的应用程序，并且可以把它们部署到云中。Openshift广泛支持多种编程语言和框架，如Java，Ruby和PHP等。另外它还提供了多种集成开发工具如Eclipse integration，JBoss Developer Studio和 Jenkins等。OpenShift 基于一个开源生态系统为移动应用，数据库服务等，提供支持。
OpenShift Online服务构建在Red Hat Enterprise Linux上。Red Hat Enterprise Linux提供集成应用程序，运行库和一个配置可伸缩的多用户单实例的操作系统，以满足企业级应用的各种需求。


# 环境准备

| hostname       | ip             | role    | username |
| -------------- | -------------- | ------- | -------- |
| master-1       | 192.168.16.190 | master  | root     |
| master-2       | 192.168.16.191 | master  | root     |
| etc-1，node1   | 192.168.16.192 | compute | root     |
| etcd-2，node2  | 192.168.16.194 | compute | root     |
| etcd-2，node-3 | 192.168.16.195 | compute | root     |

# 主机准备

1、 在master-1上创建密钥对，并分发共钥到其他机器，使用此节点作为ansible分发节点

```
ssh-keygen -t rsa # 不设置密码
ssh-copy-id -i .ssh/id_rsa.pub root@192.168.16.190
ssh-copy-id -i .ssh/id_rsa.pub root@192.168.16.191
ssh-copy-id -i .ssh/id_rsa.pub root@192.168.16.192
ssh-copy-id -i .ssh/id_rsa.pub root@192.168.16.194
ssh-copy-id -i .ssh/id_rsa.pub root@192.168.16.195
```

2、 配置各个节点的host

```
192.168.16.190 master-1 
192.168.16.191 master-2
192.168.16.192 node-1 etcd-1
192.168.16.194 node-2 etcd-2
192.168.16.195 node-3 etcd-3
```

3、 安装依赖：

ansible、docker安装`yum install ansible docker`

4、 准备`/home/okd/okd.hosts`内容如下：

```
# Create an OSEv3 group that contains the masters, nodes, and etcd groups
[OSEv3:children]
masters
nodes
etcd

# Set variables common for all OSEv3 hosts
[OSEv3:vars]
# SSH user, this user should allow ssh based auth without requiring a password
ansible_ssh_user=root
openshift_deployment_type=origin
openshift_image_tag=v3.11
# If ansible_ssh_user is not root, ansible_become must be set to true
ansible_become=true


# default selectors for router and registry services
# openshift_router_selector='node-role.kubernetes.io/infra=true'
# openshift_registry_selector='node-role.kubernetes.io/infra=true'

# uncomment the following to enable htpasswd authentication; defaults to DenyAllPasswordIdentityProvider
openshift_master_identity_providers=[{'name': 'htpasswd_auth', 'login': 'true', 'challenge': 'true', 'kind': 'HTPasswdPasswordIdentityProvider'}]
openshift_disable_check=memory_availability,disk_availability,docker_image_availability

os_sdn_network_plugin_name=redhat/openshift-ovs-multitenant

# new 2018-11--5 14:40:00
# 方便访问使用，指定web console 端口为443以及域名
openshift_master_api_port=443
openshift_master_console_port=443
openshift_hosted_router_replicas=1
openshift_hosted_registry_replicas=1
openshift_master_cluster_hostname=master.okd.xinktech.com
openshift_master_cluster_public_hostname=master.okd.xinktech.com
openshift_master_default_subdomain=okd.xinktech.com

openshift_master_cluster_method=native
openshift_public_ip=192.168.16.190
# false
ansible_service_broker_install=false
openshift_enable_service_catalog=false
template_service_broker_install=false
openshift_logging_install_logging=false

# registry passwd
#oreg_url=172.16.37.12:5000/openshift3/ose-${component}:${version}
#openshift_examples_modify_imagestreams=true

# docker config
#openshift_docker_additional_registries=172.16.37.12:5000,172.30.0.0/16
#openshift_docker_insecure_registries=172.16.37.12:5000,172.30.0.0/16
#openshift_docker_blocked_registries
openshift_docker_options="--log-driver json-file --log-opt max-size=1M --log-opt max-file=3"

# openshift_cluster_monitoring_operator_install=false
# openshift_metrics_install_metrics=true
# openshift_enable_unsupported_configurations=True
#openshift_logging_es_nodeselector='node-role.kubernetes.io/infra: "true"'
#openshift_logging_kibana_nodeselector='node-role.kubernetes.io/infra: "true"'
# host group for masters

[masters]
master-[1:2]

[etcd]
etcd-[1:3]

[nodes]
master-[1:2] openshift_node_group_name='node-config-master'
node-1 openshift_node_group_name='node-config-compute'
node-2 openshift_node_group_name='node-config-infra'
node-3 openshift_node_group_name='node-config-compute'
```
5、 分发host文件：

```
ansible -i okd.hosts  nodes --user=root -m copy -a 'src=/etc/hosts dest=/etc/hosts'
```

6、 分发CentOS-Openshift yum源

```
[okd@master-1 ~]$ cat /etc/yum.repos.d/CentOS-OpenShift-Origin311.repo
[centos-openshift-origin311]
name=CentOS OpenShift Origin
baseurl=http://buildlogs.centos.org/centos/7/paas/x86_64/openshift-origin311/
enabled=1
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-PaaS

[centos-openshift-origin311-testing]
name=CentOS OpenShift Origin Testing
baseurl=http://buildlogs.centos.org/centos/7/paas/x86_64/openshift-origin311/
enabled=0
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-PaaS

[centos-openshift-origin311-debuginfo]
name=CentOS OpenShift Origin DebugInfo
baseurl=http://debuginfo.centos.org/centos/7/paas/x86_64/
enabled=0
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-PaaS

[centos-openshift-origin311-source]
name=CentOS OpenShift Origin Source
baseurl=http://vault.centos.org/centos/7/paas/Source/openshift-origin311/
enabled=0
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-PaaS
```

分发yum源

```
[okd@master-1 ~]$ ansible -i okd.hosts  nodes --user=root -m copy -a \
'src=/etc/yum.repos.d/CentOS-OpenShift-Origin311.repo  \
dest=/etc/yum.repos.d/CentOS-OpenShift-Origin311.repo'
```

7、 下载opensift-ansible项目(需要安装git):

```
[okd@master-1 ~]$ sudo yum install -y git
[okd@master-1 ~]$ cd ~ && git clone https://github.com/openshift/openshift-ansible
# 切换到release-v3.11
[okd@master-1 ~]$ cd ~/openshift-ansible && git checkout release-3.11
```


# 安装过程

1、 安装检查:

```
[okd@master-1 ~]$ ls
okd.hosts  openshift-ansible
[okd@master-1 ~]$ ansible-playbook -i okd.hosts openshift-ansible/playbooks/prerequisites.yml
```

执行检查无误后，执行安装程序

2、 执行安装程序

```
[okd@master-1 ~]$ ansible-playbook -i okd.hosts openshift-ansible/playbooks/deploy_cluster.yml
```

安装完成后可以看到类似如下提示.可以清楚的看到安装结果，如果有失败，寻找原因再次安装:

```
PLAY RECAP ***************************************************************************************************************************************************************************************
localhost                  : ok=11   changed=0    unreachable=0    failed=0
master-1                   : ok=599  changed=198  unreachable=0    failed=0
master-2                   : ok=263  changed=72   unreachable=0    failed=0
node-1                     : ok=110  changed=16   unreachable=0    failed=0


INSTALLER STATUS *********************************************************************************************************************************************************************************
Initialization               : Complete (0:01:55)
Health Check                 : Complete (0:00:14)
Node Bootstrap Preparation   : Complete (0:04:18)
etcd Install                 : Complete (0:01:56)
Master Install               : Complete (0:13:01)
Master Additional Install    : Complete (0:02:18)
Node Join                    : Complete (0:01:16)
Hosted Install               : Complete (0:02:50)
Cluster Monitoring Operator  : Complete (0:01:21)
Web Console Install          : Complete (0:01:08)
Console Install              : Complete (0:00:52)
metrics-server Install       : Complete (0:00:03)
```

3、 查看安装结果
```
[okd@master-1 ~]$ oc get nodes
NAME       STATUS    ROLES     AGE       VERSION
master-1   Ready     master    19m       v1.11.0+d4cacc0
master-2   Ready     master    19m       v1.11.0+d4cacc0
node-1     Ready     infra     10m       v1.11.0+d4cacc0
```

4、 创建super  account,使用简单的文件密码配置登陆web控制台

```
[okd@master-1 ~]$ sudo htpasswd -cb /etc/origin/master/htpasswd admin 123456
Adding password for user admin
[okd@master-1 ~]$ oc adm policy add-cluster-role-to-user cluster-admin admin
Warning: User 'admin' not found
cluster role "cluster-admin" added: "admin"
```

文章仅供参考，安装到此完成，后续有完善或者错误继续补充。

# 错误记录：

`TASK [openshift_control_plane : Wait for control plane pods to appear]失败：`

### 错误原因分析： 

`master-api container ` 启动失败，可以看到由于etcd服务无法连接导致的，etcd cluster模式，所安装的etcd服务器通过ip无法连接，
需要增加 ansible hosts文件中：

```
[etcd]
etcd.xx.xx openshift_ip=x.x.x.x
```
重复部署安装即可


