---
title: "OpenStack下利用passthrough对GPU实现虚拟化"
date: 2018-10-27T17:51:33+08:00
categories: ["All","Linux","KVM","NVidia","openstack"]
tags: ["Linux","KVM","NVidia","openstack"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Linux","KVM","NVidia","openstack" ]
description: "OpenStack下利用passthrough对GPU实现虚拟化"
---

Tips：默认已安装好OpenStack相关必要组件并可以正常运行的情况下，本文只涉及GPU 虚拟化的相关配置说明。

## 配置GPU Passthrough的系统相关设置

1. 在`BIOS`中enable `VT-x`, `VT-d`, `Onboard VGA.` `Onboard VGA` 的enable可以避免一些错误的出现，具体参考`Not only for miners GPU integration in Nova environment.` https://www.youtube.com/watch?v=1tdvz3ejokM

2. 编辑文件 `/etc/modules`， 添加以下内容：

```
pci_stub
vfio
vfio_iommu_type1
vfio_pci
kvm
kvm_intel

```

3. 修改文件 `/etc/default/grub`：

对于Intel芯片：

`GRUB_CMDLINE_LINUX_DEFAULT="intel_iommu=on"`

对于AMD芯片：

`GRUB_CMDLINE_LINUX_DEFAULT="iommu=pt iommu=1"`

4. 运行 
   
`update-grub`

5. 将下列内容加入到blacklist中以避免被宿主机占用，编辑文件 `/etc/modprobe.d/blacklist.conf` :

```
blacklist snd_hda_intel
blacklist amd76x_edac
blacklist vga16fb
blacklist nouveau
blacklist rivafb
blacklist nvidiafb
blacklist rivatv
```

6. 查找显卡的Product ID 以及 Vendor ID：

```
root@computer1:~# lspci -nn | grep NVIDIA
04:00.0 VGA compatible controller [0300]: NVIDIA Corporation Device [10de:1b06] (rev a1)
04:00.1 Audio device [0403]: NVIDIA Corporation Device [10de:10ef] (rev a1)
05:00.0 VGA compatible controller [0300]: NVIDIA Corporation Device [10de:1b06] (rev a1)
05:00.1 Audio device [0403]: NVIDIA Corporation Device [10de:10ef] (rev a1)
```

相关参数解释参考OpenStack 企业私有云的若干需求（1）：Nova 虚机支持 GPU
http://www.cnblogs.com/sammyliu/p/5179414.html


7. 编辑文件 /etc/modprobe.d/vfio.conf:

```
# GTX 1080Ti and its audio controller
options vfio-pci ids=10de:1b06,10de:10ef

```

8. 运行：
   
`update-initramfs -u`

9. 重启服务器

10.  验证：

```
root@computer1:~$ lspci -nnk -d 10de:1b06
04:00.0 VGA compatible controller [0300]: NVIDIA Corporation Device [10de:1b06] (rev a1)
	Subsystem: Micro-Star International Co., Ltd. [MSI] Device [1462:3609]
	Kernel driver in use: vfio-pci
	Kernel modules: nvidiafb, nouveau
05:00.0 VGA compatible controller [0300]: NVIDIA Corporation Device [10de:1b06] (rev a1)
	Subsystem: Micro-Star International Co., Ltd. [MSI] Device [1462:3609]
	Kernel driver in use: vfio-pci
	Kernel modules: nvidiafb, nouveau
```
显示结果中`"Kernel driver in use: vfio-pci"`说明已经配置成功，接下来是OpenStack的配置过程。

如果经过以上步骤设备仍然被占用，可以根据文后参考资料中的最后两篇文章解绑设备。


创建虚拟机后运行`nvidia-smi`的时候如果出现｀unknown error｀错误是一定记得在KVM的xml定义文件中增加如下代码，用于隐藏显卡在虚拟机中运行：

For a Nvidia specific issue you need to add the following code snippet:
```
virsh edit vm-name
# Add to file within <features> </features> tags
<kvm>
  <hidden state='on'/>
</kvm>
```

修改完文件后记得`virsh define ×××.xml`，然后重启虚拟机即可

**Add a GPU to a VM**

After VM creation the end result is an XML file with its defined attributes such as storage, RAM, CPUs, etc. There are a few necessary additions to the XML file you have to make using the “virsh edit” command to add a GPU.

Whenever you edit the VM XML file you need to ensure that your VM is shutdown (you can do that with the virsh shutdown or virsh destroy command, the latter being a forced power off). If you don’t you could run into issues with libvirt / KVM which may result in needing to reboot your host.

For a Nvidia specific issue you need to add the following code snippet:
```
virsh edit vm-name
# Add to file within <features> </features> tags
<kvm>
  <hidden state='on'/>
</kvm>
```
This code hides the fact that this is a VM sitting on a hypervisor from the guest OS / Nvidia allowing you to properly install graphics drivers within the guest.

Then you need to actually add the code that assigns the PCI device (in our case a GPU) to the VM, an example of what that looks like:
```
# Video card
<hostdev mode='subsystem' type='pci' managed='yes'>
  <source>
    <address domain='0x0000' bus='0x4b' slot='0x00' function='0x0'/>  </source>
    <address type='pci' domain='0x0000' bus='0x00' slot='0x05' function='0x0'/>
</hostdev>
# Onboard audio
<hostdev mode='subsystem' type='pci' managed='yes'>
  <source>
    <address domain='0x0000' bus='0x4b' slot='0x00' function='0x1'/> </source>
    <address type='pci' domain='0x0000' bus='0x00' slot='0x06' function='0x0'/>
</hostdev>
```
**Note:** the part to be aware of here is the “bus=0x4b” as this relates to which PCI device you want to passthrough.
参照：
https://medium.com/@calerogers/gpu-virtualization-with-kvm-qemu-63ca98a6a172

## OpenStack的相关配置
1. 配置nova-scheduler （controller节点），编辑文件 `/etc/nova/nova.conf`:
```
[DEFAULT]
scheduler_default_filters = RetryFilter, AvailabilityZoneFilter, RamFilter, ComputeFilter, ComputeCapabilitiesFilter, ImagePropertiesFilter, ServerGroupAntiAffinityFilter, ServerGroupAffinityFilter, PciPassthroughFilter
scheduler_available_filters = nova.scheduler.filters.all_filters
```
重启nova-scheduler服务

2. 配置nova-api （controller节点），编辑文件 `/etc/nova/nova.conf`:
```
[pci]

alias = { "name": "nvidia1080", "product_id": "1b06", "vendor_id": "10de", "device_type": "type-PCI" }
```
重启nova-api服务
3. 配置nova-compute（compute 节点），编辑文件`/etc/nova/nova.conf`:

```
[pci]

passthrough_whitelist = { "vendor_id": "10de", "product_id": "1b06" }

alias = {
       "name": "nvidia1080",
       "product_id": "1b06",
       "vendor_id": "10de",
       "device_type": "type-PCI"
}
```
重启nova-compute服务


## 验证
1. 创建设置flavor：

`openstack flavor create --public --ram 2048 --disk 20 --vcpus 2 m1.large`

`openstack flavor set m1.large --property pci_passthrough:alias='nvidia1080:2'`
nvidia1080 即为alias中的那么， 2为GPU的数量。

2. 创建instance：

`openstack server create --flavor m1.large --image cirros-0.3.5-x86_64-uec --wait test-pci`
3. 在cirros下查看GPU信息如下：
```
$ lspci -k
...
00:05.0 Class 0300: 10de:1b06
00:06.0 Class 0300: 10de:1b06
...
```
## NVIDIA显卡的问题

因为NIVIDIA显卡的驱动会检测是否跑在虚拟机里，如果在虚拟机里驱动就会出错，所以我们需要对显卡驱动隐藏hypervisor id。在OpenStack的Pile版本中的Glance 镜像引入了img_hide_hypervisor_id=true的property，所以可以对镜像执行如下的命令隐藏hupervisor id：

`$ openstack image set IMG-UUID --property img_hide_hypervisor_id=true`
通过此镜像安装的instance就会隐藏hypervisor id。

如果是Pike之前的版本， 可以参考[Consumer-grade GPUs in an OpenStack system (NVIDIA GPUs)](https://gist.github.com/claudiok/890ab6dfe76fa45b30081e58038a9215)这篇文章的做法。

可以通过下边的命令查看hypervisor id是否隐藏：

```
$ cpuid | grep hypervisor_id
   hypervisor_id = "KVMKVMKVM   "
   hypervisor_id = "KVMKVMKVM   "
```
上边的显示结果说明没有隐藏，下边的显示结果说明已经隐藏：
```
$ cpuid | grep hypervisor_id
   hypervisor_id = "  @  @    "
   hypervisor_id = "  @  @    "

```