---
title: "Gerrit使用介绍"
date: 2018-09-19T13:56:48+08:00
categories: ["All","DevOps"]
tags: ["devops","code-review"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["devops","code-review" ]
description: "Gerrit使用介绍"
---

# 参照网址

http://gerrit.rockbox.org/r/Documentation/config-gerrit.html#addreviewer

https://blog.csdn.net/ujm7418529631/article/details/79226621

http://www.cnblogs.com/kevingrace/p/5651447.html

# Gerrit 权限设置
## Gerrit Tag上传权限

* 选择要编辑的项目

![选择要编辑的项目](/img/devops/gerrit/1.png)

* 选择Access选项卡，编辑项目权限

![项目Access选项卡](/img/devops/gerrit/2.png)

* 添加Reference 

![Add Reference第一步](/img/devops/gerrit/3.png)

![Add Reference第二步](/img/devops/gerrit/4.png)

* 添加Create Reference权限 

![添加Create Reference权限](/img/devops/gerrit/5.png)

* 选择Create Reference权限对应组 

![选择Create Reference权限对应组](/img/devops/gerrit/6.png)

* 添加Create Annotated Tag权限 

![添加Create Annotated Tag权限](/img/devops/gerrit/7.png)

* 选择Create Annotated Tag权限对应组 

![选择Create Annotated Tag权限对应组](/img/devops/gerrit/8.png)

* 保存权限

![保存权限](/img/devops/gerrit/9.png)

* 最终权限预览

![最终权限预览](/img/devops/gerrit/10.png)

* 上传Tag

![上传Tag](/img/devops/gerrit/11.png)