---
title: "Git clone直接提交用户名和密码"
date: 2016-08-30T10:20:07+08:00
categories: ["All","git"]
tags: ["git"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["git clone" ]
description: "Git clone直接提交用户名和密码"
---

## git使用用户名密码clone的方式：



```
git clone http://username:password@remote
```

 例如：我的用户名是`abc@qq.com`,密码是`abc123456`,git地址为`git@xxx.com/www.git`



```
git clone http://abc@qq.com:abc123456@git.xxx.com/www.git
```

 执行报错：


```
fatal: unable to access 'http://abc@qq.com:abc123456@git.xxx.com/www.git/': 
 Couldn't resolve host 'qq.com:abc123456@git.xxx.com'
```


 报错原因是因为用户名包含了@符号，所以需求要把@转码一下

```
<?php
$userame='abc@qq.com';
echo urlencode($userame);
?> 
abc%40qq.com
```


@符号转码后变成了%40，所以只需在clone时将username变为abc%40qq.com即可，再次执行就ok了。为了防止密码中也可能会有@，我觉得在拼接之前，可以对用户名和密码分别进行编码操作。