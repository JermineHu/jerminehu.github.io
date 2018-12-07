---
title: "LDAP集成多个系统配置截图"
date: 2018-12-07T09:47:45+08:00
categories: ["All"]
tags: ["",]
toc: true
author: "Jermine && Grover"
author_homepage:  "/"
weight: 70
keywords: ["", ]
description: "LDAP集成多个系统配置截图"
---

# 多个系统基于LDAP进行用户认证

OpenLDAP是一个集中的用户账号管理系统。使用轻量级目录访问协议（LDAP）构建集中的身份验证系统可以减少管理成本，增强安全性，避免数据复制的问题，并提高数据的一致性。

先看一下目录数据库吧：

![lam](/img/ldap/lam.png)

下面我们就进入正式的配置吧：

## 1.jira

![服务器设置和ldap模式](/img/ldap/jira1.png)

![用户模式设置](/img/ldap/jira2.png)

![组模式设置](/img/ldap/jira3.png)

![成员模式设置](/img/ldap/jira3.png)

## 2.云盘

![服务器](/img/ldap/pan1.png)

![用户](/img/ldap/pan2.png)

![群组](/img/ldap/pan3.png)

## 3.jenkins

![Jenkins](/img/ldap/jenkins1.png)

## 4.reviewboard

![reviewboard](/img/ldap/reviewboard1.png)

## 5.Harbor

![harbor](/img/ldap/harbor1.png)

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