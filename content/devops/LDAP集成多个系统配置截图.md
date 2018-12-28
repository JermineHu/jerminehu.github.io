---
title: "LDAP集成多个系统配置截图"
date: 2018-12-07T09:47:45+08:00
categories: ["All","DevOps","Linux"]
tags: ["docker","LDAP","Harbor","Jira","Nexcloud","Jenkins","Reviewboard","Linux"]
toc: true
author: "Jermine && Grover"
author_homepage:  "/"
weight: 70
keywords: ["docker","LDAP","Harbor","Jira","Nexcloud","Jenkins","Reviewboard","Linux" ]
description: "LDAP集成多个系统配置截图"
---

# 多个系统基于LDAP用户认证

OpenLDAP是一个集中的用户账号管理系统。使用轻量级目录访问协议（LDAP）构建集中的身份验证系统可以减少管理成本，增强安全性，避免数据复制的问题，并提高数据的一致性。

先看一下目录数据库吧：

![lam](/img/ldap/lam.png)

下面我们就进入正式的配置吧：

## 1.jira

![服务器设置和ldap模式](/img/ldap/jira1.png)

![用户模式设置](/img/ldap/jira2.png)

![组模式设置](/img/ldap/jira3.png)

![成员模式设置](/img/ldap/jira3.png)

[参考文档1](https://confluence.atlassian.com/jirakb/unable-to-login-to-jira-applications-596770904.html)

[参考文档2](https://stackoverrun.com/cn/q/3981674)

## 2.云盘

![服务器](/img/ldap/pan1.png)

![用户](/img/ldap/pan2.png)

![群组](/img/ldap/pan3.png)

## 3.jenkins

![Jenkins](/img/ldap/jenkins1.png)

## 4.reviewboard

![reviewboard](/img/ldap/reviewboard1.png)

[参考文档1](https://www.yeetrack.com/?p=934)

[参考文档2](https://blog.csdn.net/powerccna/article/details/8282250)

## 5.Harbor

![harbor](/img/ldap/harbor1.png)

[参考文档1](https://www.cnblogs.com/huangjc/p/6272938.html)

[参考文档2](https://blog.csdn.net/Laputa_SKY/article/details/80356083)

[生成自签名证书](http://blog.51cto.com/penguintux/1873025)

## 6.Linux

脚本奉上
```
#!/bin/bash
yum install -y nss-pam-ldapd nscd
authconfig --enableldap --enableldapauth --enableshadow --enablelocauthorize --ldapserver="ldap://192.168.xx.xxx" --ldapbasedn="ou=dev,dc=xinktech,dc=com" --update
echo "binddn cn=admin,dc=com" >> /etc/nslcd.conf
echo "bindpw xxxxxxx" >> /etc/nslcd.conf
systemctl restart nslcd
su - cgh
```

Ubuntu 可以参照：

```
root@www:~# apt-get -y install libnss-ldap libpam-ldap ldap-utils nscd
(1) specify LDAP server's URI
 +---------------------| Configuring ldap-auth-config |----------------------+
 | Please enter the URI of the LDAP server to use. This is a string in the   |
 | form of ldap://<hostname or IP>:<port>/. ldaps:// or ldapi:// can also    |
 | be used. The port number is optional.                                     |
 |                                                                           |
 | Note: It is usually a good idea to use an IP address because it reduces   |
 | risks of failure in the event name service problems.                      |
 |                                                                           |
 | LDAP server Uniform Resource Identifier:                                  |
 |                                                                           |
 | ldap://dlp.srv.world/_________________________________________________    |
 |                                                                           |
 |                                  <Ok>                                     |
 |                                                                           |
 +---------------------------------------------------------------------------+

(2) specify suffix
 +---------------------| Configuring ldap-auth-config |----------------------+
 | Please enter the distinguished name of the LDAP search base. Many sites   |
 | use the components of their domain names for this purpose. For example,   |
 | the domain "example.net" would use "dc=example,dc=net" as the             |
 | distinguished name of the search base.                                    |
 |                                                                           |
 | Distinguished name of the search base:                                    |
 |                                                                           |
 | dc=srv,dc=world_______________________________________________________    |
 |                                                                           |
 |                                  <Ok>                                     |
 |                                                                           |
 +---------------------------------------------------------------------------+

(3) specify LDAP version
  +---------------------| Configuring ldap-auth-config |---------------------+
  | Please enter which version of the LDAP protocol should be used by        |
  | ldapns. It is usually a good idea to set this to the highest available   |
  | version.                                                                 |
  |                                                                          |
  | LDAP version to use:                                                     |
  |                                                                          |
  |                                    3                                     |
  |                                    2                                     |
  |                                                                          |
  |                                                                          |
  |                                  <Ok>                                    |
  |                                                                          |
  +--------------------------------------------------------------------------+

(4) select the one you like. ( this example selects 'Yes' )
 +---------------------| Configuring ldap-auth-config |----------------------+
 |                                                                           |
 | This option will allow you to make password utilities that use pam to     |
 | behave like you would be changing local passwords.                        |
 |                                                                           |
 | The password will be stored in a separate file which will be made         |
 | readable to root only.                                                    |
 |                                                                           |
 | If you are using NFS mounted /etc or any other custom setup, you should   |
 | disable this.                                                             |
 |                                                                           |
 | Make local root Database admin:                                           |
 |                                                                           |
 |                    <Yes>                       <No>                       |
 |                                                                           |
 +---------------------------------------------------------------------------+

(5) select the one you like. ( this example selects 'No' )
    +-------------------| Configuring ldap-auth-config |-------------------+
    |                                                                      |
    | Choose this option if you are required to login to the database to   |
    | retrieve entries.                                                    |
    |                                                                      |
    | Note: Under a normal setup, this is not needed.                      |
    |                                                                      |
    | Does the LDAP database require login?                                |
    |                                                                      |
    |                   <Yes>                      <No>                    |
    |                                                                      |
    +----------------------------------------------------------------------+

(6) specify LDAP admin account's suffix
          +-------------| Configuring ldap-auth-config |-------------+
          | This account will be used when root changes a password.  |
          |                                                          |
          | Note: This account has to be a privileged account.       |
          |                                                          |
          | LDAP account for root:                                   |
          |                                                          |
          | cn=admin,dc=srv,dc=world_____________________________    |
          |                                                          |
          |                          <Ok>                            |
          |                                                          |
          +----------------------------------------------------------+

(7) specify password for LDAP admin account
 +---------------------| Configuring ldap-auth-config |----------------------+
 | Please enter the password to use when ldap-auth-config tries to login to  |
 | the LDAP directory using the LDAP account for root.                       |
 |                                                                           |
 | The password will be stored in a separate file /etc/ldap.secret which     |
 | will be made readable to root only.                                       |
 |                                                                           |
 | Entering an empty password will re-use the old password.                  |
 |                                                                           |
 | LDAP root account password:                                               |
 |                                                                           |
 | _________________________________________________________________________ |
 |                                                                           |
 |                                  <Ok>                                     |
 |                                                                           |
 +---------------------------------------------------------------------------+

root@www:~# vi /etc/nsswitch.conf
# line 7: add
passwd:     compat ldap
group:     compat ldap
shadow:     compat ldap
root@www:~# vi /etc/pam.d/common-password
# line 26: change ( remove 'use_authtok' )
password     [success=1 user_unknown=ignore default=die]     pam_ldap.so try_first_pass
root@www:~# vi /etc/pam.d/common-session
# add to the end if need ( create home directory automatically at initial login )
 session optional        pam_mkhomedir.so skel=/etc/skel umask=077
root@www:~# exit
Ubuntu 16.04 LTS www.srv.world ttyS0
www login: debian     # LDAP user
Password:
Welcome to Ubuntu 16.04 LTS (GNU/Linux 4.4.0-22-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

8 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

Creating directory '/home/debian'.
debian@www:~$ # just logined
debian@www:~$ passwd # try to change LDAP password
Enter login(LDAP) password:# input current password
New password:# input new password
Re-enter new password:# confirm
LDAP password information changed for debian
passwd: password updated successfully     # just changed

```

**开机启动nscd：**

```
systemctl enable nscd
```

重启




**每次修改完记得执行：** 

```
systemctl restart nscd
```



[参考文档](https://www.lisenet.com/2016/setup-ldap-authentication-on-centos-7/)


### 附录：

加证书：

```
sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain server.crt
```

[自建证书配置HTTPS服务器](https://note.youdao.com/share/?id=514cde0584119f466f528d1d0b2283de&type=note#/)

