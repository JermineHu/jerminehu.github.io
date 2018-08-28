---
title: "基于JIRA的Scrum敏捷开发的项目管理"
date: 2018-08-28T15:44:21+08:00
categories: ["All","项目管理"]
tags: ["JIRA","敏捷开发","项目管理"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Scrum","敏捷开发","项目管理" ]
description: "基于JIRA的Scrum敏捷开发的项目管理"
---

# Scrum开发的步骤及准备

Scrum敏捷开发的关键字就是增量、迭代，他更重视项目团队之间的现场沟通，不向传统瀑布式开发那样需要万事具备，才开始开发，Scrum在大方向和小故事点确认好了后，团队就可以开动了。

Scrum的团队一般都不大，一Scrum团队人数一般在10人左右，主要角色有：

product owner（产品负责人）、scrum master（团队负责人）、scrum team（开发/测试团队）。

* Product owner :需求方，提出需求，能对功能流程、业务流程拍板的人。

* Scrum master :团队负责人，负责解决团队各类问题，领导项目的人。

* Scrum team :项目执行人员，一般指项目具体开发和测试的人员。

## Scrum开发的步骤：

### 步骤一：头脑风暴

如果product owner对产品需求非常清楚，就可以省略这个步骤；开发遵守“先紧后松”原则，必须先把需求了解清楚；这里product owner可以召集技术团队/用户群体对其需求进行公开征求意见，最后输出一个产品建议表。

### 步骤二：product owner对产品建议表进行筛选并做减法，提炼最核心的需求。

在确定了需求后，由scrum master进行输出prd(product requirement document),这里就和传统的瀑布模式一样了，该有的文档都必须有，必须由scrum master和product owner确定好需求，包括业务逻辑、功能流程等。

### 步骤三：工作量估算

把任务量化，包括原型、logo设计、ui设计、前端开发等，尽量把每个工作分解到最小任务量，最小任务量标准为工作小时不能超过16小时，然后估算总体项目时间。

把每个任务都贴在白板上面，白板上分三部分：

（1）to do-待完成（2）in progress-进展中（3）done-完成

### 步骤四：Sprint

经过讨论后，已经把任务量化到需要具体完成的时间，然后把n个任务按照开发的重要度，组合成n个sprint（冲刺），每次执行一个sprint。

* Sprint：每个sprint都是独立的，一般先做主要功能，再到次要功能，再到小功能，最后的sprint一般是修复bugs。）

* Sprint：因为任务都被量化了，每天工作了多少小时，完成了多少任务量，通过每天的例会scrum master就非常清楚，并且在time burn down chart（时间燃尽表）进行表示，我们就可以直观看到任务的进度了，而且是具体到多少小时。

* Sprint：在burn down chart里面，不管任务是否按时完成都必须记录。

* Bugs：每个sprint都必须测试，尽量大家一起测试，如果太多bugs就开一个sprint来修复bugs。

* 站会：每天要做的是，要开standing meeting，因为大家的时间都是非常紧张的，一般是站着开的，时间不要长，10分钟左右为宜。会议必问开发团队每个人三个问题：（1）今天做了什么（2）明天打算做什么（3）遇到什么困难

* scrum master要解决开发团队的困难，让项目快速进展下去；每周一次周会，product owner最好在场；每个月一次月会，product owner最好在场，指出产品开发是否在product owner期待范围内；如此重复下去，直到开发完成。

（时间燃尽表：scrum的精华，通过该表格可以可视化任务的时间进度，从图中可以看到，day1是整个任务的总共时间，每天按照任务完成度更新剩余时间，或者增加时间（例如发现一个技术难点、团队成员请假等要增加开发时间））。

### 步骤五：评估

product owner和其团队/用户会对产品进行评估，可能还会有各种不满意的地方，不过product owner要求需要改的地方还是要改的，建立一个bugs sprint，把产品做到product owner最想要为止。

### 补充说明

* SCRUM也有其自身的先天缺点，就是对团队要求高，团队成员有能力且相互信任度高，不会相互推卸责任。

* 新团队使用该方法，起初会有各种问题，需要多多磨合。

 

基于JIRA的Scrum的项目管理

## 准备工作：

1、在上面的第三步时需要做工作拆分及工作量估算，会得到一个类似下面的项目计划表，JIRA的Scrum项目管理也是基于此表

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5F/wKiom1c-v6iDJFlqAAQMZajLD6s622.png)

2、团队中所有成员必须已经在JIRA中建立用户，并可以正常登陆

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5D/wKioL1c-wJfgVXUhAAJdSV2sOaY073.png)

**正式JIRA中建立Scrum开发项目**

一、建立一个Scrum的BoardsScrum的团队

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/5F/wKiom1c-v66AKFtoAAIdxyumDZY217.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5F/wKiom1c-v66AcjtTAAA9vE7MNHU078.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5F/wKiom1c-v6_xAgoeAABRCbUSyec480.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5D/wKioL1c-wJzzD2NLAABXcYxOJKM251.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/5D/wKioL1c-wJ2Qj5hrAABHhKB_6no587.png)

这是新建好的Boards,同时也建好了项目。

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5D/wKioL1c-wKGTrPBRAAGNzNms4pI662.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5F/wKiom1c-v7jh3wMxAAHNykB24Lk580.png)  

二、开发项目常规管理

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5D/wKioL1c-wKaTSws8AAGRcYKuYnc422.png)

1、项目编辑

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5D/wKioL1c-wKqyP4HuAAPoCyeIkZw927.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/5D/wKioL1c-wKuzzps2AABTpR8OS_I022.png)

2、版本开发周期设置

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/5F/wKiom1c-v8CCypgRAAIJ3TZlY20992.png)

3、添加软件开发的功能模块

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/5D/wKioL1c-wK-Sr1KFAAJIKLXN0RM456.png)

4、修改工作流

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5D/wKioL1c-wLCjdycjAACuvcqq3R4694.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5D/wKioL1c-wLCisyvGAAA_zGNO-nA135.png)

默认工作流太简单，没有QA等功能，需要重新建立工作流，或者增加一个工作流：

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/5D/wKioL1c-wLHTKERLAAC14gCqNZ4455.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5F/wKiom1c-v8XCix1NAABRf4bJLjs390.png)

这个流程比较适合Scrum项目使用，大概流程如下：建立好每个故事或子任务后，它们都处于 TO DO状态，团队成员登陆JIRA，可以看到分配给自己的任务，团队成员选择一个优先要做的任务，并把当前任务更改为IN Progress，如果遇到难题进行不下去了，就把这个任务状态改为Blocked，当哪天又可以解决的时候，再把当前任务状态改为In Progress，如果任务顺利完成，就把当前任务改成Ready For QA状态，等待进行软件测试，如果测试通过没有问题，QA就把这个任务状态改为DONE，此时这个任务就完成了。如果测试中有问题，QA会重新把任务状态改为IN PROGRESS状态，并分配处理人为开发者，同时备注问题原因，由开发者处理问题后重新提交Ready For QA。当整个Sprint都测试通过没问题，这个SPrint就结束了，但如果后来集成测试中还有问题，或者任务有了小的要求修改，相关任务，需要REOPENED，重新开始TO DO去一个新的循环。

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5D/wKioL1c-wLPhu0OHAAHJ-rvYnv0683.png)

返回项目管理中

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5D/wKioL1c-wLfg-mbGAAKHZW-3TQw163.png)

三、Scrum敏捷开发设置

1、基本设置完成后，返回可以看到功能已经全部具备，下面开始添加Story、Task了

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/5D/wKioL1c-wLnSU2qmAAHPtvh_sIg003.png)

2、建立大一些的用户故事——Epics

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5F/wKiom1c-v86QD5PZAAIGJvHd9Nc773.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5D/wKioL1c-wMSAX6ySAAB1RN1-yBs672.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5D/wKioL1c-wMWRtHbCAAAl_TAVgzY836.png)

以下设置是需要先在第一个Sprint的Planning Meeting上已经确定了Story和细分的Story Point 。

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5F/wKiom1c-v9mQMABEAACO4N4X5Tc855.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/5F/wKiom1c-v-KyqoONAAJ8Rydgo8M288.png)

3、建立第一个Sprint，并重命名，方便识别

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/60/wKiom1c-v-WQIoN-AALDvVy4YdM520.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/60/wKiom1c-v-rDAl_XAAJcKGOqHg8380.png)

4、建立story（即Scrum开发中所说的Story，如果还有子任务，这个story可以不指定经办人）

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/60/wKiom1c-v-3RqEZsAAHGHsYUpks157.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5D/wKioL1c-wNvg-m5yAAC0bOYUnxc751.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/60/wKiom1c-v--i2UmsAABrZlrWddE438.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5D/wKioL1c-wN3hcIj6AAGusrnIA-Q462.png)

选择Stroy输入Estimate（预估天数）及子任务

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/60/wKiom1c-v_XAqHHuAAIoO4Drcag986.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/5D/wKioL1c-wObQ3OC0AAM8btoK694999.png)

 

录完了所有的story后，下面按照计划表录入子任务

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5D/wKioL1c-wOyDZGf2AARmi7pfT9Y902.png)

指定每个子任务的经办人

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/60/wKiom1c-wASgPYAOAAB4lJggysM053.png)

如此方法，建立完成所有的子任务

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/60/wKiom1c-wAeS9H8CAAMAo5-yA3I652.png)

5、开始Sprint

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/60/wKiom1c-wBCjW-gTAAKCMoRQPoE803.png)

设置第一个Sprint的开始及结束时间

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5D/wKioL1c-wP7TgwnJAAFXYMTR9o4750.png)

有了活动Sprint，Active Sprint项目才能有内容。

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/60/wKiom1c-wBOCew_HAAGgrOk-jZo422.png)

在Active Sprint项目中增加Ready For QA列，用于过程测试动作的显示。

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/5D/wKioL1c-wQCgO8ILAAHz9_Ec1-g325.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5D/wKioL1c-wQKwOTT-AAITpm_lpGQ527.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5D/wKioL1c-wQTBnAEoAAIyzSQamT8591.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5D/wKioL1c-wQaidMHaAAIJI4P5Yk0040.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/5D/wKioL1c-wQnSR6a5AAFehUoBdQQ913.png)

6、设置管理面板（为了方便看到整个项目进度情况及分配 给我的任务，可以根据需要专门定制管理面板）

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5D/wKioL1c-wQzw5gbKAALxLycFLtA888.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/5D/wKioL1c-wQ7C2iXvAAFDML7jnGY781.png)

增加一个新面板，并应用给所有人

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5D/wKioL1c-wQ-gXK2AAAFbZaFiT-o986.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/60/wKiom1c-wCPDhR1IAAF-P2xZUJ0308.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5E/wKioL1c-wRKQlsNeAAJ45-I0vNw946.png)

通过增加小工具来增加工具

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5E/wKioL1c-wRKhsftUAAB18yUi7P0939.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/60/wKiom1c-wCyApMXkAADDHcOeYK4472.png)

修改及移动已有的小工具

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/60/wKiom1c-wC-hk6zJAAJa1zpqok0986.png)

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5E/wKioL1c-wR6hI9y8AAD_xWGcaTA487.png)

创建完成的面板，在用户一登陆时就会看到这个

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/60/wKiom1c-wDWAohsdAAHGo-bsXH8247.png)

项目中的6大功能板块：

一、Backlog（查看Epics-大故事，Task-小故事，Sub-Tasks-故事点。）

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5E/wKioL1c-wSSy_gNgAAKGmva1PKU566.png)

二、Active Sprints(查看进行中的Sprint的进展情况：To Do/In Progress/Done)

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/60/wKiom1c-wD2A5TTvAAF5BfgLSVY024.png)

三、Releases(版本发布情况)

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/5E/wKioL1c-wS6z7XxbAAEIJBVxasE361.png)

四、报表（各类统计报表）

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/60/wKiom1c-wEPBf5R6AAFPrUgTtXM304.png)

五、Issues（问题列表）

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5E/wKioL1c-wTfxoZ1uAALXvIkEPI0011.png)

六、模块（每个模块中的问题数量）

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5E/wKioL1c-wTuBNxgNAAEqHpoH8pI096.png)



==说明：JIRA中可以建立项目的类型（上例是建立Boards时系统自动建立的软件项目，是默认的第一个项目类型==

![p_w_picpath、](http://s3.51cto.com/wyfs02/M02/80/60/wKiom1c-wFDDNEX4AADEKcWLt44627.png)

软件类：

1、Scrum软件开发

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/60/wKiom1c-wFHQZA5eAAB7x2zT_oA294.png)

 

2、看板软件开发

![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/5E/wKioL1c-wUHAlRLJAACGo8BAVSw890.png)

3、基本软件开发

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/60/wKiom1c-wFXSFvitAAB1hZ4qb7Y605.png)

业务类：

4、任务管理

![p_w_picpath](http://s3.51cto.com/wyfs02/M01/80/5E/wKioL1c-wULCfaYXAAA3UW9saOk960.png)

5、项目管理

![p_w_picpath](http://s3.51cto.com/wyfs02/M02/80/5E/wKioL1c-wULiGyy1AABDW6_edlA547.png) 

6、过程管理

  ![p_w_picpath](http://s3.51cto.com/wyfs02/M00/80/5E/wKioL1c-wUOgRRdIAABGYnWrhhg282.png)