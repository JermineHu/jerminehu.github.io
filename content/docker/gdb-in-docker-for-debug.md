---
title: "GDB in Docker for Debug - 如何在Docker容器内部使用gdb进行debug"
date: 2017-03-08T10:02:33+08:00
categories: ["All","GDB","Docker","嵌入式"]
tags: ["嵌入式", "Docker","GDB","Debug"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["嵌入式", "Docker","GDB","Debug"]
description: "GDB in Docker for Debug - 如何在Docker容器内部使用gdb进行debug"
---

安全计算模式（secure computing mode，seccomp）是 Linux 内核功能，可以使用它来限制容器内可用的操作。


Docker 的默认 seccomp 配置文件是一个白名单，它指定了允许的调用。

下表列出了由于不在白名单而被有效阻止的重要（但不是全部）系统调用。该表包含每个系统调用被阻止的原因。

<table style="border-spacing:0px;text-align:center;color:rgb(51,51,51);font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;font-size:14px;background-color:rgb(255,255,255);"><thead><tr><th style="vertical-align:middle;">Syscall</th><th style="vertical-align:middle;">Description</th></tr></thead><tbody><tr><td style="vertical-align:middle;">acct</td><td style="vertical-align:middle;">Accounting syscall which could let containers disable their own resource limits or process accounting. Also gated by CAP_SYS_PACCT.</td></tr><tr><td style="vertical-align:middle;">add_key</td><td style="vertical-align:middle;">Prevent containers from using the kernel keyring, which is not namespaced.</td></tr><tr><td style="vertical-align:middle;">adjtimex</td><td style="vertical-align:middle;">Similar to clock_settime and settimeofday, time/date is not namespaced. Also gated by CAP_SYS_TIME.</td></tr><tr><td style="vertical-align:middle;">bpf</td><td style="vertical-align:middle;">Deny loading potentially persistent bpf programs into kernel, already gated by CAP_SYS_ADMIN.</td></tr><tr><td style="vertical-align:middle;">clock_adjtime</td><td style="vertical-align:middle;">Time/date is not namespaced. Also gated by CAP_SYS_TIME.</td></tr><tr><td style="vertical-align:middle;">clock_settime</td><td style="vertical-align:middle;">Time/date is not namespaced. Also gated by CAP_SYS_TIME.</td></tr><tr><td style="vertical-align:middle;">clone</td><td style="vertical-align:middle;">Deny cloning new namespaces. Also gated by CAP_SYS_ADMIN for CLONE_* flags, except CLONE_USERNS.</td></tr><tr><td style="vertical-align:middle;">create_module</td><td style="vertical-align:middle;">Deny manipulation and functions on kernel modules. Obsolete. Also gated by CAP_SYS_MODULE.</td></tr><tr><td style="vertical-align:middle;">delete_module</td><td style="vertical-align:middle;">Deny manipulation and functions on kernel modules. Also gated by CAP_SYS_MODULE.</td></tr><tr><td style="vertical-align:middle;">finit_module</td><td style="vertical-align:middle;">Deny manipulation and functions on kernel modules. Also gated by CAP_SYS_MODULE.</td></tr><tr><td style="vertical-align:middle;">get_kernel_syms</td><td style="vertical-align:middle;">Deny retrieval of exported kernel and module symbols. Obsolete.</td></tr><tr><td style="vertical-align:middle;">get_mempolicy</td><td style="vertical-align:middle;">Syscall that modifies kernel memory and NUMA settings. Already gated by CAP_SYS_NICE.</td></tr><tr><td style="vertical-align:middle;">init_module</td><td style="vertical-align:middle;">Deny manipulation and functions on kernel modules. Also gated by CAP_SYS_MODULE.</td></tr><tr><td style="vertical-align:middle;">ioperm</td><td style="vertical-align:middle;">Prevent containers from modifying kernel I/O privilege levels. Already gated by CAP_SYS_RAWIO.</td></tr><tr><td style="vertical-align:middle;">iopl</td><td style="vertical-align:middle;">Prevent containers from modifying kernel I/O privilege levels. Already gated by CAP_SYS_RAWIO.</td></tr><tr><td style="vertical-align:middle;">kcmp</td><td style="vertical-align:middle;">Restrict process inspection capabilities, already blocked by dropping CAP_PTRACE.</td></tr><tr><td style="vertical-align:middle;">kexec_file_load</td><td style="vertical-align:middle;">Sister syscall of kexec_load that does the same thing, slightly different arguments. Also gated by CAP_SYS_BOOT.</td></tr><tr><td style="vertical-align:middle;">kexec_load</td><td style="vertical-align:middle;">Deny loading a new kernel for later execution. Also gated by CAP_SYS_BOOT.</td></tr><tr><td style="vertical-align:middle;">keyctl</td><td style="vertical-align:middle;">Prevent containers from using the kernel keyring, which is not namespaced.</td></tr><tr><td style="vertical-align:middle;">lookup_dcookie</td><td style="vertical-align:middle;">Tracing/profiling syscall, which could leak a lot of information on the host. Also gated by CAP_SYS_ADMIN.</td></tr><tr><td style="vertical-align:middle;">mbind</td><td style="vertical-align:middle;">Syscall that modifies kernel memory and NUMA settings. Already gated by CAP_SYS_NICE.</td></tr><tr><td style="vertical-align:middle;">mount</td><td style="vertical-align:middle;">Deny mounting, already gated by CAP_SYS_ADMIN.</td></tr><tr><td style="vertical-align:middle;">move_pages</td><td style="vertical-align:middle;">Syscall that modifies kernel memory and NUMA settings.</td></tr><tr><td style="vertical-align:middle;">name_to_handle_at</td><td style="vertical-align:middle;">Sister syscall to open_by_handle_at. Already gated by CAP_SYS_NICE.</td></tr><tr><td style="vertical-align:middle;">nfsservctl</td><td style="vertical-align:middle;">Deny interaction with the kernel nfs daemon. Obsolete since Linux 3.1.</td></tr><tr><td style="vertical-align:middle;">open_by_handle_at</td><td style="vertical-align:middle;">Cause of an old container breakout. Also gated by CAP_DAC_READ_SEARCH.</td></tr><tr><td style="vertical-align:middle;">perf_event_open</td><td style="vertical-align:middle;">Tracing/profiling syscall, which could leak a lot of information on the host.</td></tr><tr><td style="vertical-align:middle;">personality</td><td style="vertical-align:middle;">Prevent container from enabling BSD emulation. Not inherently dangerous, but poorly tested, potential for a lot of kernel vulns.</td></tr><tr><td style="vertical-align:middle;">pivot_root</td><td style="vertical-align:middle;">Deny pivot_root, should be privileged operation.</td></tr><tr><td style="vertical-align:middle;">process_vm_readv</td><td style="vertical-align:middle;">Restrict process inspection capabilities, already blocked by dropping CAP_PTRACE.</td></tr><tr><td style="vertical-align:middle;">process_vm_writev</td><td style="vertical-align:middle;">Restrict process inspection capabilities, already blocked by dropping CAP_PTRACE.</td></tr><tr><td style="vertical-align:middle;">ptrace</td><td style="vertical-align:middle;">Tracing/profiling syscall, which could leak a lot of information on the host. Already blocked by dropping CAP_PTRACE.</td></tr><tr><td style="vertical-align:middle;">query_module</td><td style="vertical-align:middle;">Deny manipulation and functions on kernel modules. Obsolete.</td></tr><tr><td style="vertical-align:middle;">quotactl</td><td style="vertical-align:middle;">Quota syscall which could let containers disable their own resource limits or process accounting. Also gated by CAP_SYS_ADMIN.</td></tr><tr><td style="vertical-align:middle;">reboot</td><td style="vertical-align:middle;">Don’t let containers reboot the host. Also gated by CAP_SYS_BOOT.</td></tr><tr><td style="vertical-align:middle;">request_key</td><td style="vertical-align:middle;">Prevent containers from using the kernel keyring, which is not namespaced.</td></tr><tr><td style="vertical-align:middle;">set_mempolicy</td><td style="vertical-align:middle;">Syscall that modifies kernel memory and NUMA settings. Already gated by CAP_SYS_NICE.</td></tr><tr><td style="vertical-align:middle;">setns</td><td style="vertical-align:middle;">Deny associating a thread with a namespace. Also gated by CAP_SYS_ADMIN.</td></tr><tr><td style="vertical-align:middle;">settimeofday</td><td style="vertical-align:middle;">Time/date is not namespaced. Also gated by CAP_SYS_TIME.</td></tr><tr><td style="vertical-align:middle;">socket, socketcall</td><td style="vertical-align:middle;">Used to send or receive packets and for other socket operations. All socket and socketcall calls are blocked except communication domains AF_UNIX, AF_INET, AF_INET6, AF_NETLINK, and AF_PACKET.</td></tr><tr><td style="vertical-align:middle;">stime</td><td style="vertical-align:middle;">Time/date is not namespaced. Also gated by CAP_SYS_TIME.</td></tr><tr><td style="vertical-align:middle;">swapon</td><td style="vertical-align:middle;">Deny start/stop swapping to file/device. Also gated by CAP_SYS_ADMIN.</td></tr><tr><td style="vertical-align:middle;">swapoff</td><td style="vertical-align:middle;">Deny start/stop swapping to file/device. Also gated by CAP_SYS_ADMIN.</td></tr><tr><td style="vertical-align:middle;">sysfs</td><td style="vertical-align:middle;">Obsolete syscall.</td></tr><tr><td style="vertical-align:middle;">_sysctl</td><td style="vertical-align:middle;">Obsolete, replaced by /proc/sys.</td></tr><tr><td style="vertical-align:middle;">umount</td><td style="vertical-align:middle;">Should be a privileged operation. Also gated by CAP_SYS_ADMIN.</td></tr><tr><td style="vertical-align:middle;">umount2</td><td style="vertical-align:middle;">Should be a privileged operation. Also gated by CAP_SYS_ADMIN.</td></tr><tr><td style="vertical-align:middle;">unshare</td><td style="vertical-align:middle;">Deny cloning new namespaces for processes. Also gated by CAP_SYS_ADMIN, with the exception of unshare –user.</td></tr><tr><td style="vertical-align:middle;">uselib</td><td style="vertical-align:middle;">Older syscall related to shared libraries, unused for a long time.</td></tr><tr><td style="vertical-align:middle;">userfaultfd</td><td style="vertical-align:middle;">Userspace page fault handling, largely needed for process migration.</td></tr><tr><td style="vertical-align:middle;">ustat</td><td style="vertical-align:middle;">Obsolete syscall.</td></tr><tr><td style="vertical-align:middle;">vm86</td><td style="vertical-align:middle;">In kernel x86 real mode virtual machine. Also gated by CAP_SYS_ADMIN.</td></tr><tr><td style="vertical-align:middle;">vm86old</td><td style="vertical-align:middle;">In kernel x86 real mode virtual machine. Also gated by CAP_SYS_ADMIN.</td></tr></tbody></table>


其中gdb在进行进程debug时，会报错：

```
(gdb) attach 30721
Attaching to process 30721

ptrace: Operation not permitted.
```

原因就是因为ptrace被Docker默认禁止的问题。考虑到应用分析的需要，可以有以下几种方法解决：

1、关闭seccomp

`docker run --security-opt seccomp=unconfined`

2、采用超级权限模式
`docker run --privileged`

3、仅开放ptrace限制

`docker run --cap-add sys_ptrace`

4、server 、client 方式 （这种方式不需要设置docker本身的参数，所以更安全，更普遍，我记得android的真机调试也是server 、client方式的gdb）

使用方式可以参照：https://blog.csdn.net/zhanglianpin/article/details/80256028

当然从安全角度考虑，如只是想使用gdb进行debug的话，建议使用第三种或者第四种，第四种不需要设置任何东西。

小提示：

如果使用marathon进行docker的管理，应用JSON的修改在这里：

```
"parameters": [
        {
          "key": "cap-add",
          "value": "SYS_PTRACE"
        }
      ],
```
