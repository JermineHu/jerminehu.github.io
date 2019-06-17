---
title: "关于DevOps落地方案的个人观点"
date: 2018-09-13T18:14:57+08:00
categories: ["All","DevOps"]
tags: ["DevOps","Linux","efficiency"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["DevOps","Linux" ]
description: "关于DevOps落地方案的个人观点"
---


## 引言：

DevOps（Development和Operations的组合词）是一组过程、方法与系统的统称，用于促进开发（应用程序/软件工程）、技术运营和质量保障（QA）部门之间的沟通、协作与整合。

它是一种重视“软件开发人员（Dev）”和“IT运维技术人员（Ops）”之间沟通合作的文化、运动或惯例。透过自动化“软件交付”和“架构变更”的流程，来使得构建、测试、发布软件能够更加地快捷、频繁和可靠。

它的出现是由于软件行业日益清晰地认识到：为了按时交付软件产品和服务，开发和运营工作必须紧密合作。

可以把DevOps看作开发（软件工程）、技术运营和质量保障（QA）三者的交集。

传统的软件组织将开发、IT运营和质量保障设为各自分离的部门。在这种环境下如何采用新的开发方法（例如敏捷软件开发），这是一个重要的课题：按照从前的工作方式，开发和部署不需要IT支持或者QA深入的、跨部门的支持，而却需要极其紧密的多部门协作。然而DevOps考虑的还不止是软件部署。它是一套针对这几个部门间沟通与协作问题的流程和方法。

需要频繁交付的企业可能更需要对DevOps有一个大致的了解。Flickr发展了自己的DevOps能力，使之能够支撑业务部门“每天部署10次”的要求──如果一个组织要生产面向多种用户、具备多样功能的应用程序，其部署周期必然会很短。这种能力也被称为持续部署，并且经常与精益创业方法联系起来。 从2009年起，相关的工作组、专业组织和博客快速涌现。

DevOps的引入能对产品交付、测试、功能开发和维护（包括──曾经罕见但如今已屡见不鲜的──“热补丁”）起到意义深远的影响。在缺乏DevOps能力的组织中，开发与运营之间存在着信息“鸿沟”──例如运营人员要求更好的可靠性和安全性，开发人员则希望基础设施响应更快，而业务用户的需求则是更快地将更多的特性发布给最终用户使用。这种信息鸿沟就是最常出问题的地方。

下图中是DevOps中常用的一些工具，感兴趣的可以了解一下，虽然多到令人发指，但是针对具体领域，基本都能选出顺手的兵器，不需要全部都了解！

![devops](/img/devops/20180412212828881.png)

<center><h1>园区DevOps创新平台</h1></center>

# 概要说明

在开始介绍云思创智的DevOps创新平台之前，先对DevOps的概念进行介绍，主要从三个方面介绍DevOps。

## DevOps到底是什么呢？

### DevOps的定义

DevOps（英文Development和Operations的组合）是一组过程、方法与系统的统称，用于促进开发（应用程序/软件工程）、技术运营和质量保障（QA）部门之间的沟通、协作与整合。它的出现是由于软件行业日益清晰地认识到：为了按时交付软件产品和服务，开发和运营工作必须紧密合作,提倡开发和IT运维之间的高度协同，从而在完成高频率部署的同时，提高生产环境的可靠性、稳定性、弹性和安全性。

![DevOps](/img/devops/0b55b319ebc4b745dfdcdd5acdfc1e178a821535.jpg)


`DevOps`是一种文化转变，或者说是一个鼓励更好地交流和协作（即团队合作）以便于更快地构建可靠性更高、质量更好的软件的运动。DevOps综合了社会体系和技术体系，`DevOps`不仅仅是个工具，更是一种理念。`DevOps`是一种使持续交付成为可能的理念，关注于所有人共同协作以改进开发效率方面的衡量（比如生产力），同时增加稳定性并降低平均故障修复时间。

## DevOps几个主要部分的本质：

　　**自动化:** 自动化确保过程的可重复性和稳定性。一直以来，它都是将任务执行予以标准化的最佳方式，避免任何可能产生偏差的风险，从同行评审代码到整个团队的流程改进。

　　**透明度:** 透明度让团队中的每个成员都可以清楚地看到其他人正在做什么，正在改进的沟通机制和业务流程，等等等等。

　　**才华：** 天才雇员把业务需要、效率和自动化放到硬件如何运作之前，在IT和开发人员之间不做严格的区分。在解决问题之前，他们到处找有此类经验的同事们交流，问问他们之前是如何解决这种问题的。

![v2-d89cb453ee199af1269d284fa735c82a_r](/img/devops/v2-d89cb453ee199af1269d284fa735c82a_r.jpg)

　　`DevOps`是种与众不同的方案，它同时兼顾技术和人的问题。`DevOps`参考了许多技术方案,充分理解大多数这类实践是DevOps的基础。像持续集成此类的已经深入人心非常长的时间了，为了确保持续集成值得花时间做下去，它不但需要一台持续集成服务器还需要一致的自动化装置和验收测试。它还需要和版本控制系统紧密地集成在一起，以使所有事都在版本控制之下。除了这种技术实践之外，为了成功地实施DevOps，我们还要关注人、协作和理念。把运维融入团队中需要一种理念，那就是心甘情愿地去做出艰难地调整和改变。这是思维模式的巨大转变。

![v2-1015dcf5e27561114bb64348fdd8da03_r](/img/devops/v2-1015dcf5e27561114bb64348fdd8da03_r.jpg)

## DevOps的进化和追求

DevOps的进化如同人类进化一样，也是随着环境的变化不断的演进着。DevOps是一种思想，并不是某一个角色、某一个人、某一个职位，文档一开始中的图是一张描述DevOps最经典的一张图，其实DevOps解决的就是与开发、运维、QA交集部分的工作，其余部分大家就专注于自己的专业领域工作，将其做精做透保证自己的业务不出现问题，所以DevOps就需要整个团队加强沟通紧密合作，才能发挥最大的效果。

![2015122311](/img/devops/2015122311_ua1pt08b2.jpg)

### 早期开发状态
遥想当年，软件程序员的大部分办公司那时还被称作实验室，程序员那时还叫做科学家。为了开发出一套优秀的软件，程序员们必须深入了解他们需要的应用相关的所有问题。他们必须清楚知道这个软件应用在什么场合，这个软件是必须在什么系统上运行。本质上说，程序员对所要开发的软件的所有环节都有透彻的了解，从规格说明书编写、到软件开发、到测试、到部署、再到技术支持。

![20180524173133](/img/devops/20180524173133.png)

### 细化分工应对更多更快的常变的需求

随着软件的发展人们开始不断的进行更多的索求。比如：更快的速度，更多的功能，更多的用户，更多的所有所有。程序员这个称号就开始绝迹于江湖，而那些叫做开发者、软件工程师、网络管理员、数据库开发者、网页开发者、系统架构师、测试工程师等等更多的新物种就开始诞生。快速进化和快速适应外界的挑战成为了他们的DNA的一部分。这些新的种族可以在几个星期内就完成进化。网页开发者很快就能进化成后台开发者，前台开发者，PHP开发者，Ruby开发者，Angular开发者…多得让人侧目。

![20180524172854](/img/devops/20180524172854.png)


### 瀑布开发流程诞生

瀑布开发流程是一个非常了不起的创意，因为它利用了不同团队的开发者们只在必须的时候才进行沟通的这个事实。当一个团队完成了他们的工作的时候，它就会和下游的团队进行交流并把任务进行往下传，如此一级接一级的传递下去，永不回首。

![20180524172819](/img/devops/20180524172819.png)

### 敏捷开发流程诞生

敏捷开发最早可以追溯到1957年，伟大的约翰·冯·诺依曼和同行们的努力。但是我们最终却是等到2001年才收获到革命的果实，当时行业的十多个精英创造出了如今闻名世界的“敏捷宣言”。

敏捷宣言基于以下十二条原则：

1. 我们的首要任务是通过尽早地、持续地交付可评价的软件来使客户满意。

2. 乐于接受需求变更，即使是在开发后期也应如此。敏捷过程能够驾驭变化，从而为客户赢得竞争优势。

3. 频繁交付可使用的软件，交付间隔越短越好，可以从几个星期到几个月。

4. 在整个项目开发期间，业务人员和开发人员必须朝夕工作在一起。

5. 围绕那些有推动力的人们来构建项目。给予他们所需的环境和支持，并且信任他们能够把工作完成好。

6. 与开发团队以及在开发团队内部最快速、有效的传递信息的方法就是，面对面的交谈。

7. 可使用的软件是进度的主要衡量指标。

8. 敏捷过程提倡可持续发展。出资人、开发人员以及使用者应该总是共同维持稳定的开发速度。

9. 为了增强敏捷能力，应持续关注技术上的杰出成果和良好的设计。

10. 简洁——最大化不必要工作量的艺术——是至关重要的。

11. 最好的架构、需求和设计都源自自我组织的团队。

12. 团队应该定期反思如何能变得更有战斗力，然后相应地转变并调整其行为。

![20180524180147](/img/devops/20180524180147.png)

### 进入DevOps

DevOps通常作为下面这三个方式而为人所熟知，而在我眼里我是把它们看成是一条高速公路上的三条车道。你从第一条车道开始，然后加速进入到第二条车道，最终在第三车道上高速行驶。

**车道1 – 系统级别的整体效率考量是最主要的关注点，这超过对系统中任何一个单独个体元素的考虑**

![20180524180752](/img/devops/20180524180752.png)

**车道2 – 确保能提供持续不断的反馈循环，且这些反馈不被忽视。**

![20180524180905](/img/devops/20180524180905.png)
**车道3 – 持续的学习和吸取经验，不停的进步，快速的失败。**

![20180524180937](/img/devops/20180524180937.png)

# 云思创智DevOps平台介绍

目前云思创智有沉思者（DeepThinker）、凝瞳（DeepViewer）、微表情识别（EDA）、 沉默等多个产品正在使用我们自己研发构建的DevOps平台，所有产品的迭代从代码的提交到部署全部实现自动化，代码质量有自动化测试和代码分析进行保证，部署和运维管理采用成熟的docker、Kubernetes、SaltStack等工具；对于系统的性能监控采用Prometheus，日志分析采用EFK方案，通过性能和日志分析，定位性能瓶颈产生的根源，这样就可以进行全方位优化，消除整个系统中的缺陷，从而将整个DevOps的每个阶段形成闭环，不断的加快产品迭代速度，优化产品质量指标，达到良性循环并且稳定运行的目的。

<center><h3>DevOps流水线</h3></center>

![20180525112312](/img/devops/20180525112312.png)

# 云思创智DevOps平台核心功能与架构

## 1. 关于基础设施架构

基础设施位于所有服务的底层，为所有所有系统运行提供了必要条件。基础设施架构主要由以下几部分组成，通过对每部分的介绍进行了解。底层存储使用分布式存储系统ceph进行构建，可以满足块存储、对象存储、文件存储三大类型的存储，几乎可以满足所有场景的存储需求。应用部署采用docker容器，调度工具用Kubernetes，可以轻松部署分布式应用或微服务架构应用，可以实现自动扩容。通过监控系统可以很方便的进行服务状态查看，有利于及时调整，比如：灰度、蓝绿等发布时，需要查看服务日志、性能指标、健康状况等。

###  1.1	使用docker容器技术部署应用和服务

Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。

**采用容器技术以前会有如下问题：**
* **环境管理复杂:**

从各种OS到各种中间件到各种app, 一款产品能够成功作为开发者需要关心的东西太多，且难于管理，这个问题几乎在所有现代IT相关行业都需要面对。

* **云计算时代的到来:**  

AWS的成功, 引导开发者将应用转移到 cloud 上, 解决了硬件管理的问题，然而中间件相关的问题依然存在 (所以openstack HEAT和 AWS cloudformation 都着力解决这个问题)。开发者思路变化提供了可能性。

* **虚拟化手段的变化:**

 cloud 时代采用标配硬件来降低成本，采用虚拟化手段来满足用户按需使用的需求以及保证可用性和隔离性。然而无论是KVM还是Xen在 docker 看来,都在浪费资源，因为用户需要的是高效运行环境而非OS, GuestOS既浪费资源又难于管理, 更加轻量级的LXC更加灵活和快速.

* **LXC的移动性:**

 LXC在 linux 2.6 的 kernel 里就已经存在了，但是其设计之初并非为云计算考虑的，缺少标准化的描述手段和容器的可迁移性，决定其构建出的环境难于迁移和标准化管理(相对于KVM之类image和snapshot的概念)。docker 就在这个问题上做出实质性的革新。这是docker最独特的地方。

 面对上述几个问题，docker设想是交付运行环境如同海运，OS如同一个货轮，每一个在OS基础上的软件都如同一个集装箱，用户可以通过标准化手段自由组装运行环境，同时集装箱的内容可以由用户自定义，也可以由专业人员制造。这样，交付一个软件，就是一系列标准化组件的集合的交付，如同乐高积木，用户只需要选择合适的积木组合，并且在最顶端署上自己的名字(最后个标准化组件是用户的app)。这也就是基于docker的PaaS产品的原型。

 ![20180528101215](/img/devops/20180528101215_qn8rol73h.png)

###  1.2	容器的编排和调度工具介绍
Kubernetes（通常写成“k8s”）是最开始由google设计开发最后贡献给Cloud Native Computing Foundation的开源容器集群管理项目。它的设计目标是在主机集群之间提供一个能够自动化部署、可拓展、应用容器可运营的平台。Kubernetes通常结合docker容器工具工作，并且整合多个运行着docker容器的主机集群。

#### 历史

  `Kubernetes`（ 来自希腊语κυβερνήτης:，意思为 “操舵员” 或者 “飞行员”）由Joe Beda, Brendan Burns 和Craig McLuckie建立，并在2014年被google公司首次对外公布。它的发展和设计受到google的Borg系统的严重影响。Kubernetes项目的许多主要贡献者来自Borg项目。在Google内部Kubernetes最开始的名字叫Serven of Nine，引用了电影“星际迷航”中通常被认为“更加友好”的“博格人”这个角色。由于google律师的反对，它的名字被重命名为`Kubernetes`。从`Kubernetes`的logo上面那车轮上的七个幅条就能在一定程度上推断出Kubernets最开始的名字是什么。

  2015年七月21日`Kubernetes`发布了v1.0版本。随着`Kubernetes v1.0`版本的发布，Google和Linux基金会合作成立`Cloud Native Computing Foundation（CNCF）`并提议使`Kubernetes`成为种子技术。
  `Kubernetes`还被`RedHat`使用于`OpenShift`产品。



#### 设计
Kubernetes定义了一套堆积木，这些堆积木统一提供部署、维护和扩展应用的机制。构成Kubernetes的这些组件让Kubernetes变得一个松耦合可延伸的，因此它能满足各种不同的工作负载。Kubernetes的延展性在很大程度上是由Kubernetes的API提供的，这些API被运行在Kubernetes的内部组件、延伸组件和容器使用。

#### Pods（豆荚）
Kubernetes中的基本调度单位叫“pod”。它增加了更高层的抽象来容纳各种组件。一个pod由一个或者多个容器组成，这些容器能够部署在同一台物理主机上面，并能够共享资源。Kubernetes中集群内部的每一个pod被指定了唯一的IP地址，用户程序可以通过相应的端口号无冲突地连接各个pod。pod能够定义一个卷（volume），比如一个本地磁盘目录或者一个网络磁盘，然后把它暴露给pod中的容器。用户可以通过Kubernetes API手动管理pod，或者把管理工作交给一个管理器。

####  标签和选择器
Kubernetes可以让客户端（用户或者内部组件）把被称之为标签的键值对依附在系统的任何API对象上，比如pods和“nodes”。相应地，”标签选择器”是针对标签的查询，这些标签用于解决匹配对象问题。

标签和选择器是Kubernetes中的主要分组机制，用来决定哪个操作应用于哪个组件。

比如，如果一个应用的pod有一个系统标签为：`tier ("front-end", "back-end",) `和 `release_track ("canary", "production")`， 然后所有 `back-end` 和`canary`节点 上的操作都可以使用如下所示的标签选择器：

`tier=back-end AND release_track=canary`

####  控制器
一个控制器是一个调节回路，通过管理一系列pod来驱动实际的集群状态变成所需的集群状态。一种控制器叫”复制控制器“，通过运行指定数目的跨集群的pod副本来进行复制和扩展操作。如果底层的节点失败了，它还能处理和创建用于替换的pod。其他的控制器是核心Kubernetes系统的一部分，包括一个运行在所有机器（或者所有机器的一些子集）但恰好一个pod上的”DaemonSet“控制器，以及一个运行pod直到结束的”Job“控制器（比如，作为批作业的一部分）。控制器所管理的那一系列pod由定义在控制器里的部分标签选择器决定。

#### 服务
一个Kubernetes服务是一系列工作在一起的pod，比如多层应用中其中的一层。这一系列pod构成了由标签选择器所定义的一个服务。Kubernetes提供了服务发现和请求路由的功能。请求路由是通过分配固定IP地址和DNS名字给服务。默认的，一个服务会在一个集群内暴露（比如，后台的pod会被分到一个服务中，来自前端的pods负载平衡他们之间的请求），但是，它也可以在一个集群外暴露（比如，为客户端访问前端的pod）。

#### 架构
![Kubernetes](/img/devops/Kubernetes.png)

Kubernetes采用了主从架构。Kubernetes的组件可以被分为那些管理单个节点和那些控制平面（control plane）的部分。

####  Kubernetes控制平面（plane）
Kubernetes的master主要是在不同系统之间负责管理工作负载和指导通信的控制单元。Kubernetes的控制平面由不同的组件组成，它们自己的进程可以运行在一个单独的master节点上，或者运行在由多个master所支持的高可用集群中。Kubernetes控制平面的不同组件如下所示：

#### Etcd
etcd 是一个由CoreOS开发的轻量级的、分布式的key-value数据存储器。它能够可靠地存储集群的配置数据和展现整个集群在某一时间点的状态。其他的组件监视着这个存储器的变化情况以便更新所需的状态。

#### API server
API Server是一个关键组件，它在HTTP协议上使用JSON为Kubernetes对内外提供kubernetes API服务。API server处理和验证REST请求和更新etcd中API对象的状态，因此，这使得客户端能够在各个worker节点上配置工作负载和容器。

#### Scheduler
Scheduler是一个可插拔的组件，它能够根据资源的可用性决定一个还没被调度的pod应该运行在哪个节点上面。Scheduler追踪每个节点的资源使用情况，确保将调度的资源不超出剩下可用的资源。为了达到这个目的，scheduler必须知道可用资源的情况和在各个服务器上已经分配的资源情况。

#### Controller manager
controller manager是核心Kubernetes控制器（比如DaemonSet控制器、复制控制器）所运行的进程。这些控制器跟API服务器通信来创建、更新和删除它们所管理的资源（pod、service端点等等）

#### Kubernetes node
节点（Node）（也叫worker或者minion）是部署着容器的单个机器（或者虚拟机）。集群中的每一个节点必须运行着容器运行时（runtime）（比如Docker）以及下面所提到的组件，用来和master通信以便让这些容器进行网络配置

#### Kubelet
Kubelet负责每个节点的运行状态，也就是说确保节点中的所有容器正常运行。它会按照控制平面（plane）的指示启动、停止和维护容器（组织成pods）。

Kubelet监视一个pod的状态，如果没有看到想要的状态，那么这个pod会被重新部署到同一个节点上。节点的状态依赖于每几秒所发送给master的心跳信息。当master侦测到一个节点失败了，复制控制（Replication Controller）就会知道这个状态改变了，然后会在另一个正常的节点上启动相应的pod。

#### Kube-proxy
kube-proxy是网络代理和负载均衡的实现。它和其他的网络操作提供了服务抽象。它负责根据ip地址和端口号来路由外部请求到相应的容器。

#### cAdvisor
cAdvisor是监听和收集资源使用情况和性能指标（比如每个节点中容器的CPU、内存和网络使用情况）的代理者。

### 1.3	深度学习基础镜像构建
 对于深度学习中的4大主流框架（Tensorflow、Mxnet、Pytorch、Caffe/Caffe2）封装成基础镜像，通过Nvidia Docker 和Kubernetes的插件可以非常方便的在容器中构造一个完善的训练环境或运行环境，这样就可以通过容器技术的轻量化虚拟技术实现环境隔离进程隔离，从而完美解决了裸机下对不同软件、同一软件版本、不同版本驱动等一系列的环境问题，为开发人员和运维人员解决了基础环境的问题，这样就节约了大量的配置环境的时间，只需要关注业务实现即可，因此可以加速产品的迭代效率，为后续的工作赢得更多时间。

 基础镜像托管于hub.docker.com之上，构建docker镜像的Dockerfile源码托管于Github，因为做了自动构建的集成，每次升级Dockerfile都会自动生成新的镜像，并且根据tag会保留对应版本的基础镜像，从而保证了基础镜像的稳定和可追踪，以及构建镜像所定义的Dockerfile的版本控制。

 基于定制的深度学习基础镜像，在多个项目中只需要从基础镜像去配置所依赖的环境，比如某个python项目中用到多个依赖包，只需要增加python环境和执行pip install即可，这样项目根目录下的Dockerfile就非常简洁，阅读和修改就比较方便，另外就是不需要花大量的时间去编译和安装基础镜像中的环境，能节省很多时间又能解决环境问题。

### 1.4	Docker仓库建设方案

#### Harbor介绍
 docker仓库的管理工具采用Vmware的企业级开源产品Harbor。Habor是由VMWare公司开源的容器镜像仓库。事实上，Habor是在Docker Registry上进行了相应的企业级扩展，从而获得了更加广泛的应用，这些新的企业级特性包括：管理用户界面，基于角色的访问控制 ，AD/LDAP集成以及审计日志等。

 容器的核心在于镜象的概念，由于可以将应用打包成镜像，并快速的启动和停止，因此容器成为新的炙手可热的基础设施CAAS，并为敏捷和持续交付包括DevOps提供底层的支持。

 **主要组件包括:**

 * **Proxy：** 对应启动组件nginx,它是一个nginx反向代理，代理Notary client（镜像认证）、Docker client（镜像上传下载等）和浏览器的访问请求（Core Service）给后端的各服务；
  * **UI（Core Service）：** 对应启动组件harbor-ui。底层数据存储使用mysql数据库，主要提供了四个子功能：
  * **UI：** 一个web管理页面ui；
  * **API：** Harbor暴露的API服务；
  * **Auth：** 用户认证服务，decode后的token中的用户信息在这里进行认证；auth后端可以接db、ldap、uaa三种认证实现；
  * **Token服务（上图中未体现）：** 负责根据用户在每个project中的role来为每一个docker push/pull命令issuing一个token，如果从docker client发送给registry的请求没有带token，registry会重定向请求到token服务创建token。
  * **Registry：** 对应启动组件registry,负责存储镜像文件，和处理镜像的pull/push命令。Harbor对镜像进行强制的访问控制，Registry会将客户端的每个pull、push请求转发到token服务来获取有效的token。
 * **Admin Service:** 对应启动组件harbor-adminserver。是系统的配置管理中心附带检查存储用量，ui和jobserver启动时候需要加载adminserver的配置；
  * **Job Sevice：** 对应启动组件harbor-jobservice。负责镜像复制工作的，他和registry通信，从一个registry pull镜像然后push到另一个registry，并记录job_log；
  * **Log Collector：** 对应启动组件harbor-log。日志汇总组件，通过docker的log-driver把日志汇总到一起；
  * **Volnerability Scanning：** 对应启动组件clair,负责镜像扫描
  * **Notary：** 对应启动组件notary,负责镜像认证
  * **DB：** 对应启动组件harbor-db，负责存储project、 user、 role、replication、image_scan、access等的metadata数据。

上面这些就是Docker Registry所完成的主要工作，而Habor在此之上，又提供了用户、同步等诸多特性，这篇文章中我们就这几个方面作一些阐述，同时实例代码介绍Harbor与K8s的集成。

<center> <h4>Harbor架构 </h4></center>

![20180321084347144](/img/devops/20180321084347144.png)

#### Harbor的安全机制

企业中的软件研发团队往往划分为诸多角色，如项目经理、产品经理、测试、运维等。在实际的软件开发和运维过程中，这些角色对于镜像的使用需求是不一样的。从安全的角度，也是需要通过某种机制来进行权限控制的。

在Harbor中，用户主要分为两类。一类为管理员，另一类为普通用户。两类用户都可以成为项目的成员。而管理员可以对用户进行管理。

成员是对应于项目的概念，分为三类：管理员、开发者、访客。管理员可以对开发者和访客作权限的配置和管理。测试和运维人员可以访客身份读取项目镜像，或者公共镜像库中的文件。
从项目的角度出发，显然项目管理员拥有最大的项目权限，如果要对用户进行禁用或限权等，可以通过修改用户在项目中的成员角色来实现，甚至将用户移除出这个项目。

<center> <h4>用户管理图示 </h4></center>

![20170314213441](/img/devops/20170314213441.jpg)

#### Harbor的镜像同步

<h5>为什么需要镜像同步</h5>
由于对镜像的访问是一个核心的容器概念，在实际使用过程中，一个镜像库可能是不够用的，下例情况下，我们可能会需要部署多个镜像仓库：

<p>

* 国外的公有镜像下载过慢，需要一个中转仓库进行加速
<br/>
* 容器规模较大，一个镜像仓库不堪重负
<br/>
* 对系统稳定性要求高，需要多个仓库保证高可用性
<br/>
* 镜像仓库有多级规划，下级仓库依赖上级仓库
<br/>

</p>

更常用的场景是，在企业级软件环境中，会在软件开发的不同阶段存在不同的镜像仓库。

<p>

* 在开发环境库，开发人员频繁修改镜像，一旦代码完成，生成稳定的镜像即需要同步到测试环境。
* 在测试环境库，测试人员对镜像是只读操作，测试完成后，将镜像同步到预上线环境库。
* 在预上线环境库，运维人员对镜像也是只读操作，一旦运行正常，即将镜像同步到生产环境库。
* 在这个流程中，各环境的镜像库之间都需要镜像的同步和复制。

</p>   

<h5>Harbor的镜像同步机制</h5>

有了多个镜像仓库，在多个仓库之间进行镜像同步马上就成为了一个普遍的需求。比较传统的镜像同步方式，有两种：

* 第一种方案，使用Linux提供的RSYNC服务来定义两个仓库之间的镜像数据同步。
* 第二种方案，对于使用IaaS服务进行镜像存储的场景，利用IaaS的配置工具来对镜像的同步进行配置。

这两种方案都依赖于仓库所在的存储环境，而需要采用不同的工具策略。Harbor则提供了更加灵活的方案来处理镜像的同步，其核心是三个概念：

* 用Harbor自己的API来进行镜像下载和传输，作到与底层存储环境解耦。
* 利用任务调度和监控机制进行复制任务的管理，保障复制任务的健壮性。在同步过程中，如果源镜像已删除，Harbor会自动同步删除远端的镜像。在镜像同步复制的过程中，Harbor会监控整个复制过程，遇到网络等错误，会自动重试。
* 提供复制策略机制保证项目级的复制需求。在Harbor中，可以在项目中创建复制策略，来实现对镜像的同步。与Docker Registry的不同之处在于，Harbor的复制是推（PUSH）的策略，由源端发起，而Docker Registry的复制是拉（PULL）的策略，由目标端发起。

![20170314213448](/img/devops/20170314213448.jpg)

<h5>Harbor的多级部署</h5>
在实际的企业级生产运维场景，往往需要跨地域，跨层级进行镜像的同步复制，比如集团企业从总部到省公司，由省公司再市公司的场景。
这一部署场景可简化如下图：

![20170314213455](/img/devops/20170314213455.jpg)

更复杂的部署场景如下图：

![20170314213503](/img/devops/20170314213503.jpg)

可以采用Kubernetes部署Harbor，并通过secret设置认证信息进行Harbor私有仓库登陆，从而完成私有镜像的拉取或者部署。

### 1.5	Kubernetes中应用程序部署和升级方案

Kubernetes中应用程序部署和升级采用helm和operator进行简化，使用Git仓库作为helm的chart管理能很好的做包的版本控制。

#### 使用Helm部署应用程序都k8s

对于一些微服务架构来说，会有不同的服务在上面运行，你可能要管理诸如deployment、service、有状态的Statefulset、权限的控制等等。你会发现，部署应用后还会有很多其他关联的东西以及你需要考虑的点：比如说你的不同团队，要去管理这样一个应用，从开发到测试再到生产，在不同的环境中，同样一套东西可能都需要不同的配置。例如，你在开发的时候，不需要用到PV，而是用一些暂时的存储就行了；但是在生产环境中，你必须要持久存储；并且你可能会在团队之间做共享，然后去存档。

另外，你不仅仅要部署这个应用资源，你还要去管理其生命周期，包括升级、更新换代、后续的删除等。我们知道，K8S里面的deployment是有版本管理的，但是从整个应用或某个应用模块来考虑的话，除了deployment，可能还会有其他的configmap之类的去跟其关联。这时我们会想，是否有这样一个工具可以在更上层的维度去管理这些应用呢？这个时候我们就有了社区的一个包管理工具：Helm。

我们知道K8S的意思是舵手，即掌控船舵的那个人。而Helm其实就是那个舵。在Helm里面，它的一个应用包叫Charts，Charts其实是航海图的意思。它其实就是一个应用的定义描述。里面包括了这个应用的一些元数据，以及该应用的K8S资源定义的模板及其配置。其次，Charts还可以包括一些文档的说明，这些可以存储在chart的仓库里面。 Helm分为服务端跟客户端两部分，你在helm init之后，它会把一个叫做Tiller的服务端，部署在K8S里面。这个服务端可以帮你管理Helm Chart应用包的一个完整生命周期。


<center> <h5>Chart 的安装实例 </h5></center>

![20180528162417](/img/devops/20180528162417.png)

chart的仓库其实就是一个HTTP服务器。只要你把你的chart以及它的索引文件放到上面，在Helm install的时候，就可以通过上面的路径去拿。

Helm工具本身也提供一个简单的指令，叫Helm serve，帮你去做一个开发调试用的仓库。

例如 https://example.com/charts 的仓库目录结构：

![20180528162645](/img/devops/20180528162645.png)

关于 Helm，社区版其实已经有了很多的应用包，一般放在K8S下面的一些项目中，比如安装Helm时候，它默认就有一个Stable的项目。里面会有各种各样的应用包。Stable和incubator chart 仓库：https://github.com/kubernetes/charts


另外，社区版还会提供类似于Rancher Catalog应用商店的这样一个概念的UI，你可以在这上面做管理。它叫Monocular，即单筒望远镜的意思，这些项目的开发都非常的活跃，一直在随着K8S的迭代做着更新。

Monocular: chart的UI管理项目：https://github.com/kubernetes-helm/monocular

![20180528162803](/img/devops/20180528162803.png)

Helm是插件化的，helm的插件有Helm-templates, helm-github，等等。比如你在Helm install的时候，它可以调用插件去做扩展。它没有官方的仓库，但是已经有一些功能可用。其实是把Restless/release的信息以及你的chart信息以及Tiller的连接信息交给插件去处理。Helm本身不管插件是用什么形式去实现的，只要它是应用包，则对传入的这些参数做它自己的处理就行。

Helm的好处，大概就有这些：

* 利用已有的Chart快速部署进行实验

* 创建自定义Chart，方便地在团队间共享

* 便于管理应用的生命周期

* 便于应用的依赖管理和重用

* 将K8S集群作为应用发布协作中心

#### Operator

Operator其实并不是一个工具，而是为了解决一个问题而存在的一个思路。什么问题？就是我们在管理应用时，会遇到无状态和有状态的应用。管理无状态的应用是相对来说比较简单的，但是有状态的应用则比较复杂。在Helm chart的stable仓库里面，很多数据库的chart其实是单节点的，因为分布式的数据库做起来会较为麻烦。

Operator的理念是希望注入领域知识，用软件管理复杂的应用。例如对于有状态应用来说，每一个东西都不一样，都可能需要你有专业的知识去处理。对于不同的数据库服务，扩容缩容以及备份等方式各有区别。能不能利用K8S便捷的特性去把这些复杂的东西简单化呢？这就是Operator想做的事情。

拿etcd来说，它是K8S里面主要的存储。如果对它做一个Scale up的话，需要往集群中添加一些新节点的连接信息，从而获取到集群的不同Member的配置连接。然后用它的集群信息去启动一个新的etcd节点。

如果有了etcd Operator，会怎么样？Operator其实是CoreOS布道的东西。CoreOS给社区出了几个开源的Operator，包括etcd，那么如何在这种情况下去扩容一个etcd集群？

首先可以以deployment的形式把etcd Operator部署到K8S中。部署完这个Operator之后，想要部署一个etcd的集群，其实很方便。因为不需要再去管理这个集群的配置信息了，你只要告诉我，你需要多少的节点，你需要什么版本的etcd，然后创建这样一个自定义的资源，Operator会监听你的需求，帮你创建出配置信息来。

`$ kubectl create –f etcd-cluster.yaml`

![20180528163710](/img/devops/20180528163710.png)

要扩容的话也很简单，只要更新数量（比如从3改到5），再apply一下，它同样会监听这个自定义资源的变动，去做对应的更新。

`$ kubectl apply -f upgrade-example.yaml`

![20180528164031](/img/devops/20180528164031.png)

这样就相当于把以前需要运维人员去处理集群的一些工作全部都交付给Operator去完成了。etcd的做法是在拉起一个etcd Operator的时候，创建一个叫etcd cluster的自定义资源，监听应用的变化。比如你的声明你的更新，它都会去产生对应的一个事件，去做对应的更新，将你的etcd集群维护在这样的状态。除了etcd以外，社区比如还有普罗米修斯Operator都可以以这种方便的形式，去帮你管理一些有状态的应用。

#### Helm和Operator的对比

Operator本质上是针对特定的场景去做有状态服务，或者说针对拥有复杂应用的应用场景去简化其运维管理的工具。Helm的话，它其实是一个比较普适的工具，想法也很简单，就是把你的K8S资源模板化，方便共享，然后在不同的配置中重用。

其实Operator做的东西Helm大部分也可以做。用Operator去监控更新etcd的集群状态，也可以用定制的Chart做同样的事情。只不过你可能需要一些更复杂的处理而已，例如在etcd没有建立起来时候，你可能需要一些init Container去做配置的更新，去检查状态，然后把这个节点用对应的信息给拉起来。删除的时候，则加一些PostHook去做一些处理。所以说Helm是一个更加普适的工具。两者甚至可以结合使用，比如stable仓库里就有etcd-operator chart。在K8S这个庞然大物之上，他们两者都诞生于简单自然的想法，helm是为了配置分离，operator则是针对复杂应用的自动化管理。

### 1.6	实现K8s调度GPU节点和容器内调用GPU计算方案

要实现K8s调度到GPU节点，需要先安装Nvidia的k8s插件，同时容器需要支持GPU显卡的调用，因此还需要在GPU节点上安装nvidia的驱动和Nvidia Docker的Runtime环境。具体操作可以如下方法：


#### 1、安装cuda 和 gpu的驱动程序

**cuda下载地址如下：**
 https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=runfilelocal

或者

https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=debnetwork

**安装 nvidia的docker**

```
# Add the package repositories
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update

# Install nvidia-docker2 and reload the Docker daemon configuration
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd

# Test nvidia-smi with the latest official CUDA image
docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi
```

**或者采用离线安装方式：**

```

wget https://nvidia.github.io/libnvidia-container/ubuntu16.04/amd64/libnvidia-container1_1.0.0~alpha.3-1_amd64.deb
wget https://nvidia.github.io/libnvidia-container/ubuntu16.04/amd64/libnvidia-container-tools_1.0.0~alpha.3-1_amd64.deb
wget https://nvidia.github.io/nvidia-container-runtime/ubuntu16.04/amd64/nvidia-container-runtime_1.1.1+docker17.12.0-1_amd64.deb
wget https://nvidia.github.io/nvidia-docker/ubuntu16.04/amd64/nvidia-docker2_2.0.2+docker17.12.0-1_all.deb

sudo dpkg -i *.deb

```

#### 2、设置GPU节点的docker的配置文件/etc/docker/daemon.json，使其默认运行时使用nvidia


```
cat > /etc/docker/daemon.json
{
    "registry-mirrors": [
        "https://2nmcv9vp.mirror.aliyuncs.com"
    ],
    "insecure-registries": [],
    "default-runtime": "nvidia",
    "runtimes": {
        "nvidia": {
            "path": "/usr/bin/nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}
```

如果节点使用的是kubeadm进行部署的，需要在/etc/systemd/system/kubelet.service.d/10-kubeadm.conf 增加一行

```
Environment="KUBELET_EXTRA_ARGS=--feature-gates=DevicePlugins=true"
```

然后重新加载systemd的配置并且重启kubelet

```
$ sudo systemctl daemon-reload
$ sudo systemctl restart kubelet
```

#### 3、安装Nvidia的k8s插件


```

# 仓库地址：https://github.com/NVIDIA/k8s-device-plugin

# For Kubernetes v1.8
kubectl create -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v1.8/nvidia-device-plugin.yml

# For Kubernetes v1.9
kubectl create -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v1.9/nvidia-device-plugin.yml

```

#### 4、通过插件调用GPU

```
apiVersion: v1
kind: Pod
metadata:
  name: cuda-vector-add
spec:
  restartPolicy: OnFailure
  containers:
    - name: cuda-vector-add
      # https://github.com/kubernetes/kubernetes/blob/v1.7.11/test/images/nvidia-cuda/Dockerfile
      image: "k8s.gcr.io/cuda-vector-add:v0.1"
      resources:
        limits:
          nvidia.com/gpu: 1 # requesting 1 GPU
```

如果有不同的显卡可以通过标签进行标识，然后通过节点选择器进行选择

```
# Label your nodes with the accelerator type they have.
kubectl label nodes <node-with-k80> accelerator=nvidia-tesla-k80
kubectl label nodes <node-with-p100> accelerator=nvidia-tesla-p100

```

然后定义发布的yml文件

```
apiVersion: v1
kind: Pod
metadata:
  name: cuda-vector-add
spec:
  restartPolicy: OnFailure
  containers:
    - name: cuda-vector-add
      # https://github.com/kubernetes/kubernetes/blob/v1.7.11/test/images/nvidia-cuda/Dockerfile
      image: "k8s.gcr.io/cuda-vector-add:v0.1"
      resources:
        limits:
          nvidia.com/gpu: 1
  nodeSelector:
    accelerator: nvidia-tesla-p100 # or nvidia-tesla-k80 etc.

```

### 1.7	私有Git仓库建设
  私有Git仓库使用Gitea搭建。官方给出的解释是 Gitea: Git with a cup of tea . Gitea 是一个开源社区驱动的 Gogs 克隆, 是一个轻量级的代码托管解决方案，后端采用 Go 编写，采用 MIT 许可证.Gitea 是一个自己托管的Git服务程序。他和GitHub, Bitbucket or Gitlab等比较类似。

<h4>采用Gitea的原因</h4>

**开源化:** 所有代码均在 GitHub 采用 MIT 许可证

**易安装:** 直接 从二进制安装。 或者使用 Docker， Vagrant， 和 安装包.

**跨平台:** Gitea 可以运行在任何 Go 能够编译的平台：Windows, macOS, Linux, ARM 等等，选择你喜欢的即可！

**轻量级:** Gitea 拥有很低的系统需求，即使Raspberry Pi也可运行，节约机器资源！

<h4>功能特性</h4>

* 支持活动时间线
* 支持 SSH 以及 HTTP/HTTPS 协议
* 支持 SMTP、LDAP 和反向代理的用户认证
* 支持反向代理子路径
* 支持用户、组织和仓库管理系统
* 支持添加和删除仓库协作者
* 支持仓库和组织级别 Web 钩子（包括 Slack 集成）
* 支持仓库 Git 钩子和部署密钥
* 支持仓库工单（Issue）、合并请求（Pull Request）以及 Wiki
* 支持迁移和镜像仓库以及它的 Wiki
* 支持在线编辑仓库文件和 Wiki
* 支持自定义源的 Gravatar 和 Federated Avatar
* 支持邮件服务
* 支持后台管理面板
* 支持 MySQL、PostgreSQL、SQLite3, MSSQL 和 TiDB（实验性支持） 数据库
* 支持多语言本地化（21 种语言）

### 1.8	实时视频流rtmp、hls服务器方案

SRS定位是运营级的互联网直播服务器集群，追求更好的概念完整性和最简单实现的代码。SRS提供了丰富的接入方案将RTMP流接入SRS，包括推送RTMP到SRS、推送RTSP/UDP/FLV到SRS、拉取流到SRS。SRS还支持将接入的RTMP流进行各种变换，譬如将RTMP流转码、流截图、转发给其他服务器、转封装成HTTP-FLV流、转封装成HLS、转封装成HDS、录制成FLV。SRS包含支大规模集群如CDN业务的关键特性，譬如RTMP多级集群、VHOST虚拟服务器、无中断服务Reload、HTTP-FLV集群、Kafka对接。此外，SRS还提供丰富的应用接口，包括HTTP回调、安全策略Security、HTTP API接口、RTMP测速。

 云思创智采用SRS作为服务端，客户端采用深度学习框架+opencv+ffmpeg进行处理，并输出到SRS服务器，实现对视频的实时处理。

<h4>采用SRS的原因</h4>

*  遵循m3u8 / ts规范完全重写HLS，并且HLS支持h.264 + aac / mp3。
*  高效RTMP交付支持7k +并发性，基于虚拟主机，起源和边缘。
*  嵌入式简化媒体HLS，api和HTTP flv / ts / mp3 / aac流式HTTP服务器。
*  各种输入：RTMP，通过摄取文件或流（HTTP / RTMP / RTSP）拉，通过流推送器RTSP / MPEGTS-over-UDP。
*  流行的互联网传送：用于Flash的RTMP / HDS，用于移动的HLS（IOS / IPad / MAC / Android），用户喜好的HTTP flv / ts / mp3 / aac流。
*  增强的DVR和hstrs：段/会话/附加计划，客户路径和HTTP回调。hstrs（http流触发rtmp源）启  用http-flv流备用util编码器启动发布，类似于rtmp，这将触发边缘从原始获取。
*  多功能：转码，转发，摄取，HTTP挂钩，dvr，hls，rtsp，http流，http api，引用，日志，带宽测试和srs-librtmp。
*  最好的维护：对于状态线程（协同程序），单线程，单进程和linux / osx平台，通用服务器x86-64 / i386 / arm / mips cpus，丰富的注释，严格遵循RTMP / HLS / RTSP规范。
*  易于使用：英文和中文wiki，通常在trunk / conf中配置文件，可追踪和基于会话的日志，linux服务脚本和安装脚本。
*  麻省理工学院许可证，开放源代码与产品管理和演变

### 1.9	性能监控方案

 性能监控方案采用Prometheus等工具，图形化用grafana。Prometheus是由 SoundCloud 开发的开源监控报警系统和时序列数据库(TSDB).自2012年起,许多公司及组织已经采用 Prometheus,并且该项目有着非常活跃的开发者和用户社区.现在已经成为一个独立的开源项目核,并且保持独立于任何公司,Prometheus 在2016加入 CNCF ( Cloud Native Computing Foundation ), 作为在 kubernetes 之后的第二个由基金会主持的项目.

<h4>Prometheus 的特点</h4>

和其他监控系统相比，Prometheus的特点包括：

* 多维数据模型（时序列数据由metric名和一组key/value组成）
* 在多维度上灵活的查询语言(PromQl)
* 不依赖分布式存储，单主节点工作.
* 通过基于HTTP的pull方式采集时序数据
* 可以通过中间网关进行时序列数据推送(pushing)
* 目标服务器可以通过发现服务或者静态配置实现
* 多种可视化和仪表盘支持

<h4>Prometheus 相关组件</h4>

Prometheus生态系统由多个组件组成，其中许多是可选的：

* Prometheus 主服务,用来抓取和存储时序数据
* client library 用来构造应用或 exporter 代码 (go,java,python,ruby)
* push 网关可用来支持短连接任务
* 可视化的dashboard (两种选择,promdash 和 grafana.目前主流选择是 grafana.)
* 一些特殊需求的数据出口(用于HAProxy, StatsD, Graphite等服务)
* 实验性的报警管理端(alartmanager,单独进行报警汇总,分发,屏蔽等 )

promethues 的各个组件基本都是用 golang 编写,对编译和部署十分友好.并且没有特殊依赖.基本都是独立工作

<center><h4>Prometheus架构 </h4></center>

![2018050309150095](/img/devops/2018050309150095.png)

Grafana是一个可视化面板（Dashboard），有着非常漂亮的图表和布局展示，功能齐全的度量仪表盘和图形编辑器，支持Graphite、zabbix、InfluxDB、Prometheus和OpenTSDB作为数据源。Grafana主要特性：灵活丰富的图形化选项；可以混合多种风格；支持白天和夜间模式；多个数据源。

![20180528175143](/img/devops/20180528175143.png)

![20180528175155](/img/devops/20180528175155.png)

### 1.10	日志收集和处理，以及监控和预警

 日志收集和处理采用EFK系统,Elasticsearch,Fluentd,Kibana三个组合起来简称EFK。

 Elasticsearch是个开源分布式搜索引擎，提供搜集、分析、存储数据三大功能。它的特点有：分布式，零配置，自动发现，索引自动分片，索引副本机制，restful风格接口，多数据源，自动搜索负载等。

 Fluentd是一个日志收集系统，通过丰富的插件，可以收集来自于各种系统或应用的日志，然后根据用户定义将日志做分类处理。

 Kibana 也是一个开源和免费的工具，Kibana可以为 Fluent 和 ElasticSearch 提供的日志分析友好的 Web 界面，可以帮助汇总、分析和搜索重要数据日志。

### 1.11	应用程序实现分布式高可用架构方案
 当前云思创智的产品架构选择微服务架构（实现Service Mesh的Istio），通过消息队列实现程序间解耦，protobuf作为序列化，grpc作为调用协议 。

 随着微服务架构的普及，越来越多的应用已经拆分成了微服务的架构。而微服务架构落地的一个难点，就是如何让服务和服务之间进行稳定的通信。

 部署微服务之后，如何做服务的负载均衡、容错性、服务监控、日志追踪以及熔断等功能都需要考虑周全。在写完微服务之后，引入了更多的组件，带来了极大的部署复杂度，每个组件都需要高可用、负载均衡、熔断、日志收集等等。为了让业务团队返璞归真，将所有精力集中在业务代码而不是配合微服务组件写大量非功能性需求的代码，Istio应运而生。

Istio是谷歌、IBM、Lyft等公司贡献的开源Service Mesh组件。它实现的目标就是让业务开发不再关注微服务之间如何调用、管理、监控等非功能性需求，而是让Istio来处理这些问题。Istio和Kubernetes有天然的支持。

Istio能轻松解决蓝绿发布和金丝雀发布的问题。金丝雀发布有什么用？它的最大实际意义，是让运维不用在夜里加班上线，而是能够在白天工作时间进行上线。先上线1%的节点，如果失败了，立刻回滚。

Istio提供Control Plane，让服务和服务之间能够实现实时请求，并且支持了自定义的路由规则、重试机制、熔断机制、性能监控以及追踪。而之前这些事情都是由程序员处理（Spring Cloud组件）。

使用Istio之后，再来看我们的微服务组件架构图，你会发现，之前的API网关、服务注册中心、负载均衡、熔断等组件都不需要了，这些都由Istio来处理，Istio就是要来取代部门Spring Cloud的功能。所以你的微服务会只剩下服务本身和一个代理（SideCar）。Istio使用Envoy作为代理实现服务的动态发现、负载均衡、熔断等等。Envoy是基于C++开发的4/7层代理。

![20180528181139](/img/devops/20180528181139.png)

在Kubernetes集群里，你可以为每个微服务通过一个Policy文件描述代理服务，Istio Pilot会统一收集所有的代理注册信息，用来进行服务之间的请求调度。Istio的调度机制如下：

* Service Mesh解析所有的请求，并把这些请求发送到本地的代理；

* 负载均衡会将请求分发到某个代理实例；

* 代理实例会进行检查，例如ACL、Quota等等；

* 如果成功，目标服务返回结果给请求调用者。

![20180528181225](/img/devops/20180528181225.png)

所有的服务追踪信息可以统一通过Istio Mixer进行收集，并发送到Prometheus，用Grafana进行数据可视化展示。你可以在你的Istio路由规则里配置根据User-Agent的关键字，做流量的规划，而不需要修改任何应用的代码。通过配置本次发布的节点数，能够智能地让Istio来分配比如2%的流量给V2版本，做金丝雀发布的验证，如果成功了，再自动进行10%节点的升级和流量调度。

Istio基本实现了Spring Cloud的很多非功能性的需求，如果是将微服务部署到Kubernetes集群，你可以使用Istio来简化你的Spring Cloud组件，为你的微服务运维减轻负担，让业务团队将更多的精力放在业务代码上。

如果想快速试用Istio，最好的方法是使用Kubernetes Helm Chart进行一键Istio部署："helm install incubator/istio"，即可在你的Kubernetes集群里部署Istio服务。

### 1.12	分布式存储系统方案
 分布式存储系统方案采用Ceph，为k8s提供持久化支持与海量存储。

<h4>CEPH 简介</h4>
 不管你是想为云平台提供Ceph 对象存储和/或 Ceph 块设备，还是想部署一个 Ceph 文件系统或者把 Ceph 作为他用，所有 Ceph 存储集群的部署都始于部署一个个 Ceph 节点、网络和 Ceph 存储集群。 Ceph 存储集群至少需要一个 Ceph Monitor 和两个 OSD 守护进程。而运行 Ceph 文件系统客户端时，则必须要有元数据服务器（ Metadata Server ）。

![20180529145225](/img/devops/20180529145225.png)

* **Ceph OSDs:** Ceph OSD 守护进程（ Ceph OSD ）的功能是存储数据，处理数据的复制、恢复、回填、再均衡，并通过检查其他OSD 守护进程的心跳来向 Ceph Monitors 提供一些监控信息。当 Ceph 存储集群设定为有2个副本时，至少需要2个 OSD 守护进程，集群才能达到 active+clean 状态（ Ceph 默认有3个副本，但你可以调整副本数）。
* **Monitors:** Ceph Monitor维护着展示集群状态的各种图表，包括监视器图、 OSD 图、归置组（ PG ）图、和 CRUSH 图。 Ceph 保存着发生在Monitors 、 OSD 和 PG上的每一次状态变更的历史信息（称为 epoch ）。
* **MDSs:** Ceph 元数据服务器（ MDS ）为 Ceph 文件系统存储元数据（也就是说，Ceph 块设备和 Ceph 对象存储不使用MDS ）。元数据服务器使得 POSIX 文件系统的用户们，可以在不对 Ceph 存储集群造成负担的前提下，执行诸如 ls、find 等基本命令。

Ceph 把客户端数据保存为存储池内的对象。通过使用 CRUSH 算法， Ceph 可以计算出哪个归置组（PG）应该持有指定的对象(Object)，然后进一步计算出哪个 OSD 守护进程持有该归置组。 CRUSH 算法使得 Ceph 存储集群能够动态地伸缩、再均衡和修复。

### 1.13	分布式Redis的集群方案
 分布式Redis的集群方案采用Codis 或者pika实现。

#### Codis方案

Codis 是一个用golang编写的分布式 Redis 解决方案, 对于上层的应用来说, 连接到 Codis Proxy 和连接原生的 Redis Server 没有明显的区别 (不支持的命令列表), 上层应用可以像使用单机的 Redis 一样使用, Codis 底层会处理请求的转发, 不停机的数据迁移等工作, 所有后边的一切事情, 对于前面的客户端来说是透明的, 可以简单的认为后边连接的是一个内存无限大的 Redis 服务.

**组成部分**

* Codis Proxy (codis-proxy)

codis-proxy 是客户端连接的 Redis 代理服务, codis-proxy 本身实现了 Redis 协议, 表现得和一个原生的 Redis 没什么区别 (就像 Twemproxy), 对于一个业务来说, 可以部署多个 codis-proxy, codis-proxy 本身是无状态的.

* Codis Manager (codis-config)

codis-config 是 Codis 的管理工具, 支持包括, 添加/删除 Redis 节点, 添加/删除 Proxy 节点, 发起数据迁移等操作. codis-config 本身还自带了一个 http server, 会启动一个 dashboard, 用户可以直接在浏览器上观察 Codis 集群的运行状态.

* Codis Redis (codis-server)

codis-server 是 Codis 项目维护的一个 Redis 分支, 基于 2.8.13 开发, 加入了 slot 的支持和原子的数据迁移指令. Codis 上层的 codis-proxy 和 codis-config 只能和这个版本的 Redis 交互才能正常运行.

* ZooKeeper

Codis 依赖 ZooKeeper 来存放数据路由表和 codis-proxy 节点的元信息, codis-config 发起的命令都会通过 ZooKeeper 同步到各个存活的 codis-proxy.

Codis 支持按照 Namespace 区分不同的产品, 拥有不同的 product name 的产品, 各项配置都不会冲突.

**特性**

* 自动平衡
* 使用非常简单
* 图形化的面板和管理工具
* 支持绝大多数 Redis 命令，完全兼容twemproxy
* 支持 Redis 原生客户端
* 安全而且透明的数据移植，可根据需要轻松添加和删除节点
* 提供命令行接口
* RESTful APIs

#### Pika方案

pika是360 DBA和基础架构组联合开发的类redis 存储系统, 完全支持Redis协议，用户不需要修改任何代码, 就可以将服务迁移至pika。有维护redis经验的DBA，维护pika 不需要学习成本。pika 主要解决的是用户使用redis的内存大小超过50G, 80G 等等这样的情况, 会遇到比如启动恢复时间长, 一主多从代价大, 硬件成本贵, 缓冲区容易写满等等问题。

Pika是一个可持久化的大容量redis存储服务，兼容string、hash、list、zset、set的绝大部分接口(兼容详情)，解决redis由于存储数据量巨大而导致内存不够用的容量瓶颈，并且可以像redis一样，通过slaveof命令进行主从备份，支持全同步和部分同步，pika还可以用在twemproxy或者codis中来实现静态数据分片。（pika已经可以支持codis的动态迁移slot功能，目前在合并到master分支）

**特点**
* 容量大，支持百G数据量的存储
* 兼容redis，不用修改代码即可平滑从redis迁移到pika
* 支持主从(slaveof)
* 完善的运维命令

### 1.14	RDB的方案
 RDB的方案采用Newsql方案，如比较流行的cockroach 或tidb ，两者都兼容传统数据库协议。cockroach 兼容Postgresql协议，tidb兼容Mysql协议。所以当数据增长很快需要无限横向扩展时，这两个NewSql方案是个不错的选择。

#### NewSQL起源
对于MySQL、Oracle、PostgreSQL这样的单机数据库，随着数据量的增长在计算容量和存储容量上都会出现问题。于是后续又推出了基于中间件或者NoSQL的方案，但是都并非完美，比如中间件在分布式事务方面以及NoSQL在SQL接口和对事务的支持方面做了一定退让。

2011年分析师Matthew Aslett首次提出了NewSQL的概念，期望将NoSQL和传统的数据库的优势融合，将现有数据库存在的缺陷在下一代中解决掉。而Google首先将这一概念工程化，也就是Spanner。随后开源社区也陆续跟进。


#### CockroachDB方案

 CockroachDB，目标是打造一个开源、可伸缩、跨地域复制且兼容事务的 ACID 特性的分布式数据库，它不仅能实现全局（多数据中心）的一致性，而且保证了数据库极强的生存能力，就像 Cockroach（蟑螂）这个名字一样，是打不死的小强。

 CockroachDB 的思路源自 Google 的全球性分布式数据库 Spanner。其理念是将数据分布在多数据中心的多台服务器上，实现一个可扩展，多版本，全球分布式并支持同步复制的数据库。

 Cockroach DB于2014年托管在GitHub，遵循Apache License，基于Golang实现。 Star数量12000+，Contributor数量150+。当前2.0.1版本。母公司是Cockroach Labs，公司的三位创始人全部来自Google，有Big Table，GFS，Colossus，Gmail项目背景，已获得来自Benchmark，Google Venture等共计5325万的融资。总部位于纽约，目前有50+员工。

**Cockroach DB架构**

Cockroach DB采用类似Spanner的分层架构，在分布式KV上提供了SQL引擎，分布式KV之下引入了自身独有三个概念Node、Store、Range。

* **Node & Store:** Node是Cockroach DB的进程实例，一台物理服务器启动一个Node即可，一个物理存储介质（例如一块硬盘）一般配置一个Store，一个Node中有多个Store。

* **Range:** Range是Cockroach DB存储管理的最小单位，一个Range是一段键值区间的数据分片。一个Store中有多个Range，每个Range分片默认为64M，默认存在3个副本，分布在不同的Node上。

**典型应用场景**

Cockroach DB比较适合OLTP场景，同时支持轻量级别OLAP场景。这些场景有如下特点：

- 高并发读写，支持多点写入，自动负载均衡

- 大数据量存储

- 随时按需扩展、在线扩容

- 跨数据中心容灾，多副本数据强一致

- 时延要求不苛刻

#### TiDB方案

TiDB 是 PingCAP 公司受 Google Spanner / F1 论文启发而设计的开源分布式 HTAP (Hybrid Transactional and Analytical Processing) 数据库，结合了传统的 RDBMS 和 NoSQL 的最佳特性。TiDB 兼容 MySQL，支持无限的水平扩展，具备强一致性和高可用性。TiDB 的目标是为 OLTP (Online Transactional Processing) 和 OLAP (Online Analytical Processing) 场景提供一站式的解决方案。

**TiDB 具备如下核心特性：**

* **高度兼容 MySQL**

大多数情况下，无需修改代码即可从 MySQL 轻松迁移至 TiDB，分库分表后的 MySQL 集群亦可通过 TiDB 工具进行实时迁移。

* **水平弹性扩展**

通过简单地增加新节点即可实现 TiDB 的水平扩展，按需扩展吞吐或存储，轻松应对高并发、海量数据场景。

* **分布式事务**

TiDB 100% 支持标准的 ACID 事务。

* **真正金融级高可用**

相比于传统主从 (M-S) 复制方案，基于 Raft 的多数派选举协议可以提供金融级的 100% 数据强一致性保证，且在不丢失大多数副本的前提下，可以实现故障的自动恢复 (auto-failover)，无需人工介入。

* **一站式 HTAP 解决方案**

TiDB 作为典型的 OLTP 行存数据库，同时兼具强大的 OLAP 性能，配合 TiSpark，可提供一站式 HTAP 解决方案，一份存储同时处理 OLTP & OLAP，无需传统繁琐的 ETL 过程。

* **云原生 SQL 数据库**

TiDB 是为云而设计的数据库，同 Kubernetes 深度耦合，支持公有云、私有云和混合云，使部署、配置和维护变得十分简单。

TiDB 的设计目标是 100% 的 OLTP 场景和 80% 的 OLAP 场景，更复杂的 OLAP 分析可以通过 TiSpark 项目来完成。

TiDB 对业务没有任何侵入性，能优雅的替换传统的数据库中间件、数据库分库分表等 Sharding 方案。同时它也让开发运维人员不用关注数据库 Scale 的细节问题，专注于业务开发，极大的提升研发的生产力。

### 1.15	分布式消息队列方案
 分布式消息队列方案通过k8s实现RabbitMQ集群，用于分布式消息队列。

#### RabbitMQ
 RabbitMQ是流行的开源消息队列系统，用erlang语言开发。RabbitMQ是AMQP（高级消息队列协议）的标准实现。 MQ全称为Message Queue, 消息队列（MQ）是一种应用程序对应用程序的通信方法。应用程序通过读写出入队列的消息（针对应用程序的数据）来通信，而无需专用连接来链接它们。消息传递指的是程序之间通过在消息中发送数据进行通信，而不是通过直接调用彼此来通信，直接调用通常是用于诸如远程过程调用的技术。排队指的是应用程序通过 队列来通信。队列的使用除去了接收和发送应用程序同时执行的要求。

概念说明：
* Broker：简单来说就是消息队列服务器实体。
* Queue：消息队列载体，每个消息都会被投入到一个或多个队列。
* Exchange：消息交换机，它指定消息按什么规则，路由到哪个队列。
* Binding：绑定，它的作用就是把exchange和queue按照路由规则绑定起来。
* Routing Key：路由关键字，exchange根据这个关键字进行消息投递。
* vhost：虚拟主机，一个broker里可以开设多个vhost，用作不同用户的权限分离。
* producer：消息生产者，就是投递消息的程序。
* consumer：消息消费者，就是接受消息的程序。
* channel：消息通道，在客户端的每个连接里，可建立多个channel，每个channel代表一个会话任务。

消息队列的使用过程大概如下：

（1）客户端连接到消息队列服务器，打开一个channel。
（2）客户端声明一个exchange，并设置相关属性。
（3）客户端声明一个queue，并设置相关属性。
（4）客户端使用routing key，在exchange和queue之间建立好绑定关系。
（5）客户端投递消息到exchange。

exchange接收到消息后，就根据消息的key和已经设置的binding，进行消息路由，将消息投递到一个或多个队列里。

exchange也有几个类型，完全根据key进行投递的叫做Direct交换机，例如，绑定时设置了routing key为”abc”，那么客户端提交的消息，只有设置了key为”abc”的才会投递到队列。对key进行模式匹配后进行投递的叫做Topic交换机，符号”#”匹配一个或多个词，符号”*”匹配正好一个词。例如”abc.#”匹配”abc.def.ghi”，”abc.*”只匹配”abc.def”。还有一种不需要key的，叫做Fanout交换机，它采取广播模式，一个消息进来时，投递到与该交换机绑定的所有队列。

RabbitMQ支持消息的持久化，也就是数据写在磁盘上，为了数据安全考虑，我想大多数用户都会选择持久化。消息队列持久化包括3个部分：

（1）exchange持久化，在声明时指定durable => 1
（2）queue持久化，在声明时指定durable => 1
（3）消息持久化，在投递时指定delivery_mode => 2（1是非持久化）

如果exchange和queue都是持久化的，那么它们之间的binding也是持久化的。如果exchange和queue两者之间有一个持久化，一个非持久化，就不允许建立绑定。

**Kubernets部署RabbitMQ集群**

准备好RabbitMQ的相关镜像以Kubernets的StatefulSet类型部署RabbitMQ集群，可以使用Ceph的块存储RBD作为存储卷，将RabbitMQ的数据保存在Ceph RBD中。 这就需要对我们的Kubernetes和Ceph集群做一些准备工作，需要在Ceph中创建专门给Kubernetes使用的存储池，同时配置Kubernetes的Node节点访问Ceph，并在Kubernetes上创建StorageClass。

当使用Kubernetes作为rabbitmq-autocluster的backend时，autocluster会通过访问Kubernetes的API Server获取RabbitMQ服务的endpoints，这样就能拿到Kubernete集群中的RabbitMQ的Pod的信息，从而可以将它们添加到RabbitMQ的集群中去。 这里也就是说要在autocluster实际上是在RabbitMQ Pod中要访问Kubernetes的APIServer。

因为已经对Kubernetes的API Server启用了TLS认证，同时也为API Server起到用了RBAC，要想从Pod中访问API Server需要借助Kubernetes的Service Account。 Service Account是Kubernetes Pod中的程序用于访问Kubernetes API的Account(账号)，它为Pod中的程序提供访问Kubernetes API的身份标识。可以先创建rabbitmq Pod的ServiceAccount，并针对Kubernetes的endpoint资源做授权，创建相关的role和rolebinding。通过定义好的yaml文件和环境变量的设置，然后启动Service和StatefulSet，保证RabbitMQ节点之间可以通信，这样就可以在RabbitMQ Management中查看RabbitMQ的n个节点已经组成了集群。

### 1.16	针对企业内部管理系统的用户实现统一管理方案

 针对企业内部管理系统的用户实现统一管理方案采用OpenLDAP实现对用户的统一管理，避免每套系统重新录入用户的重复工作,并且能够很好实现多个系统之间的用户管理模块的集成，达到用户管理的统一目的。

OpenLDAP是轻型目录访问协议（LDAP）的自由和开源的实现，在其OpenLDAP许可证下发行，并已经被包含在众多流行的Linux发行版中。

LDAP是轻量目录访问协议，英文全称是Lightweight Directory Access Protocol，一般都简称为LDAP。它是基于X.500标准的，但是简单多了并且可以根据需要定制。与X.500不同，LDAP支持TCP/IP，这对访问Internet是必须的。LDAP的核心规范在RFC中都有定义，所有与LDAP相关的RFC都可以在LDAPman RFC网页中找到。

OpenLDAP主要包括下述4个部分：

* slapd - 独立LDAP守护服务
* slurpd - 独立的LDAP更新复制守护服务
* 实现LDAP协议的库
* 工具软件和示例客户端


## 2.	代码质量控制
### 2.1.代码提交管理流程
代码提交管理流程使用Git flow约定代码管理流程。能够满足发布、跟踪、开发等不同任务需求的源代码管理。

#### Git flow 分支一般分5种类型

* **master分支**
主分支，产品的功能全部实现后，最终在master分支对外发布。

* **develop分支**
开发分支，基于master分支克隆，产品的编码工作在此分支进行。

* **release分支**
测试分支，基于delevop分支克隆，产品编码工作完成后，发布到本分支测试，测试过程中发现的小bug直接在本分支进行修复，修复完成后合并到develop分支。本分支属于临时分支，目的实现后可删除分支。

* **bugfix分支**
Bug修复分支，基于master分支或发布的里程碑Tag克隆，主要用于修复对外发布的分支，收到客户的Bug反馈后，在此分支进行修复，修复完毕后分别合并到develop分支和master分支。本分支属于临时分支，目的实现后可删除分支。

* **feature分支**
功能特征分支，基于develop分支克隆，主要用于多人协助开发场景或探索性功能验证场景，功能开发完毕后合并到develop分支。feature分支可创建多个，属于临时分支，目的实现后可删除分支。

<center><h3>基本流程如下图</h3></center>

![20180306141732512](/img/devops/20180306141732512.png)

### 2.2.	Code Review方案
采用Gerrit做Code Review，可以设定code Review的人数，控制代码提交流程，保证代码质量，同时强制成员互相审核代码，从而达到所有成员都能了解整个项目开发的目的，而不是每个人只了解自己所开发的模块。

Gerrit实际上是一个Git服务器，它为在其服务器上托管的Git仓库提供一系列权限控制，以及一个用来做Code Review是Web前台页面。当然，其主要功能就是用来做Code Review，如果为了保证主库中的代码每个提交都可以正常运行，可以在自动化测试和人工审核后通过后，设置Gerrit同步到远程仓库的配置信息，这样就可以将review后的高质量源代码推送到远程Git仓库。

### 2.3.	源代码静态分析方案
源代码静态分析方案采用Sonarqube作为代码的静态分析工具，可以将Bug和风险从源码端规就避掉一大部分，节省了测试人员和开发人员开发完以后再去寻找潜在安全问题、性能问题、规范等问题。

静态代码分析就是从源码中找出代码存在的缺陷：潜在的bug，未使用的代码，复杂的表达式，重复的代码等。

SonarQube 是一个开源的代码分析平台, 用来持续分析和评测项目源代码的质量。 通过SonarQube我们可以检测出项目中重复代码， 潜在bug， 代码风格问题，缺乏单元测试、注释率不足或过、糟糕的复杂度分布高等问题， 并通过一个web ui展示出来。

<center><h4>持续集成中使用SonarQube</h4></center>

主要步骤如下：

1. 用户本地使用IDE的插件进行代码分析
2. 用户上传到源代码版本控制服务器
3. 持续集成，使用Sonar Scanner进行扫描
4. 将扫描结果上传到SonarQube服务器
5. SonarQube server将结果写入db
6. 用户通过web ui查看扫描结果
7. SonarQube导出结果到其他需要的服务

![002391c59bf6b0947a56cdcd7c67ea67](/img/devops/002391c59bf6b0947a56cdcd7c67ea67.png)

### SonarQube的架构

SonarQube平台包含了四个组件：分析器，服务端，安装在服务端的插件以及数据库。


![u=4151372887,3768929876&fm=173&s=4984D6120F6A470914DC2549030030F2&w=519&h=297&img](/img/devops/sonarqube-Component.JPEG)

分析器负责一行一行地执行代码分析。它能够提供技术债务，代码覆盖率，代码复杂度，检测出的问题等信息。检查出来的问题可以是bug，潜在的bug，或者是可能在将来导致错误的问题等。当分析完成后，结果可以通过由SonarQube服务端提供的网页进行浏览。SonarQube的web服务器简化了SonarQube实例的配置，插件的安装等过程，并提供一个直观的结果概览图。

![u=3558783782,3505219959&fm=173&s=1D285D320B2341224A5DB0DA0000C0B1&w=640&h=280&img](/img/devops/sonarqube-scan.JPEG)

在代码检查中可以发现很多问题。不同的规则基于他们的严重程度可以放入以下5组中的任意一组： Blocker, Critical, Major, Minor 和 Info。一旦某条规则发现了bug或潜在的bug，这些bug就会被标记为Blocker或者Critical，一些小的问题例如“魔数不应该被使用”则会被标记为Minor 或Info。



### 2.4.	设定多分枝Pipeline构建
每个repo都设定Jenkins multibranch pipeline作为CI的创建类型，方便对不同分支的代码进行测试和构建，保证不同分支的代码质量。

**Jenkins 2.0新特性**

**Pipeline**

Jenkins2.0支持三种类型的Pipeline:普通Pipeline，MultibranchPipeline和Organization Folders。后两种其实是批量创建一组普通Pipeline的快捷方式，分别对应于多分支的应用和多应用的大型组织。

**Pipeline设计三个基本概念：**

* Stage: 一个Pipeline可以划分为若干个Stage，每个Stage代表一组操作。注意，Stage是一个逻辑分组的概念，可以跨多个Node。

* Node: 一个Node就是一个Jenkins节点，或者是Master，或者是Agent，是执行Step的具体运行期环境。

* Step:Step是最基本的操作单元，小到创建一个目录，大到构建一个Docker镜像，由各类Jenkins Plugin提供。

Multibranch Plugin

该插件通过指定托管仓库，提供Jenkinsfile后，系统会自动监控该仓库上的分支，通过配置规则对满足条件的分支运行Jenkinsfile脚本，实现项目版本的构建、部署、测试、发布等pipeline环节。

![239db11360e246c1844d74e57177c4d7](/img/devops/239db11360e246c1844d74e57177c4d7.jpeg)


## 3.	实现CI/CD自动化方案
### 3.1.	CI/CD的工具选择
 我们采用Jenkins，因其插件多、开源、使用广泛方便；
### 3.2.	多分枝构建流水线的配置实现
 采用MulitBrach Pipeline + Jenkinsfile进行构建流水线的配置，这样做的好处就是更加灵活并且支持多分枝构建，根据需要可以通过Groovy定制自己的流程，如果流水线有变动的话只需要改动项目目录下的Jenkinsfile即可触发并且改变相应构建流程，对于流水线的控制，不但快而且可以准确把控；
### 3.3.	通过Pipeline中的stag定义通知事件
 对于Pipeline的stage根据需求加入IM通知，由于我们工作流采用Dingtalk，所以就会将Jenkins的构建信息推送到钉钉工作组，方便不通阶段通知不同群组的人，至于为什么不用Slack，BarryChat、Email等，因为Dingtalk足够实现消息通知，不需要再增加额外工具,少即是多。不用Email 是因为没有即时工具及时，而且不能即时与相关人员在线讨论，沟通起来太繁琐，IM工具可以很直观简洁的表达；
### 3.4.	定义CI/CD流程：
 流水线的流向大致为：用户本地git review后提交代码   ->  Gerrit库  -> 执行Sonarqube对代码进行静态分析（从源码就开始注重代码质量） ->  触发Jenkins自动测试, 测试通过标记为Verified  ->  人工审核Review，review通过  -> Gerrit执行Replication  ->   push Git remote -> 触发远程仓库下的Jenkins Pipeline->Build docker image -> Push Docker image to Harbor -> Deploy to test environment for test from Harbor -> Run E2E test -> 将结果通知给当前代码分支的负责人（架构师、测试经理或发布人员等）->管理员将代码merrage到 release 分支-> 进过充分测试和准备后由管理人员合并到Master 分支->触发发布程序（调用K8s的API进行灰度发布或者蓝绿发布） ；

 <center><h3>CI/CD序列图</h3></center>

![image](//ws2.sinaimg.cn/large/006LbLfvgy1g0zv3097hbj31dh0ou43r.jpg)

此图采用mermaid生成，源码如下：

 ```mermaid
 sequenceDiagram
   participant CodeCommit
   participant Gerrit
   participant Jenkins
   participant Sonarqube
   participant Harbor
   participant Kubernetes
   participant Docker
   participant Gitea
   participant Dingtalk
   CodeCommit->>Gerrit: 提交代码到Gerrit
   Note right of Gerrit: 触发Jenkins调用Sonarqube进行代码静态分析
   Gerrit-->>Jenkins: 触发Jenkins开始执行Pipeline
   loop CodeCheck
         Jenkins->>Sonarqube: 执行代码静态分析，直到通过
     end
   loop CodeCheck
         Jenkins->>Jenkins: 触发单元测试、集成测试等测试, 测试通过标记为Verified  
     end
   Jenkins--> Dingtalk: 自动化测试通过后，通过Dingtalk即时通知管理员去Gerrit Review代码
   Gerrit-->> Gitea: Gerrit执行Replication 将代码push到正式的仓库Gitea
   Gitea-->> Jenkins: 触发Jenkins执行代码目录下的Jenkinsfile（MulitBrach Pipeline）
   Jenkins-->> Docker: 构建Docker images
   Jenkins-->>Harbor: 将构建好的Docker镜像推入Harbor仓库
   Jenkins-->Kubernetes: 从Harbor部署到测试环境
   Jenkins-->Dingtalk: 执行E2E测试，将结果通知给当前代码分支的负责人（架构师、测试经理或发布人员等）
   Gitea-->Gitea: 管理员将代码Merrage到 release 分支(同样会触发多分枝构建，执行上述流程，发布到release环境，即预发布环境)
   Gitea-->Gitea: 进过充分测试和准备后由管理人员合并到Master分支
   Gitea-->Jenkins: 触发发布程序
   Jenkins-->Kubernetes:调用K8s的API进行发布，让程序灰度到生产
 ```

 <center><h3>CI/CD流程图</h3></center>

  ![image](//ws1.sinaimg.cn/large/006LbLfvgy1g0zv3vyl07j31040p7jvt.jpg)

  此图采用mermaid生成，源码如下：

 ```mermaid
 graph TB
 开发((开发提交代码))-->Gerrit;
 Gerrit--触发构建-->Jenkins[在Jenkins构建];
 Jenkins--Sonarqube静态态分析-->测试[自动化测试];
 测试--将提交代码标记为Verified<br>通知管理员人工审核代码-->DingTalk{DingTalk根据事件<br>通知不同组成员};
 DingTalk--测试报告-->开发组;
 DingTalk--Jira任务-->项目组["包含产品、测试、开发等成员"];
 DingTalk--管理员Review-->Review通过;
 Review通过--push到远程Git仓库-->Gitea;
 Gitea--触发多分枝构建-->Jenkins
 Jenkins--构建docker镜像-->Harbor
 Jenkins--推送即时消息--> DingTalk
 Jenkins--调用Kubernets API<br>部署到测试环境-->Kubernets{Kubernets根据namepace<br>部署到不同的环境}
 Kubernets-->测试环境
 Kubernets-->生产环境
 Kubernets-->预发布环境
 Jenkins--执行E2E测试-->测试环境
 Jenkins--E2E测试结果-->DingTalk
 DingTalk--"将结果通知给当前代码分支的<br>负责人（架构师、测试经理或发布人员等）"--> 代码仓库管理员
 Gitea((远程代码仓库Gitea))--管理员将dev分支代码merrage到<br>release分支-->Gitea
 Gitea--release分支经过充分测试和准备后由管理人员合并到Master分支-->Gitea
 生产环境-->状态{运行状态}
 状态--成功-->发布结束
 状态--"发布失败，就回滚到上一个版本"-->Kubernets
 ELK--App服务日志收集和分析-->测试环境
 ELK--App服务日志收集和分析-->生产环境
 ELK--App服务日志收集和分析-->预发布环境
 Prometheus--性能监控-->Kubernets
 ```



## 4. 基于容器和私有云部署深度学习方案
### 4.1.	借助云计算和大数据加速深度学习和训练大模型的能力

对于深度学习方面，可借助k8s通过大数据工具（spark、storm等工具）和机器学习框架实现分布式训练，加快训练速度，加强通过海量数据训练大模型的能力。

**一般训练模型的痛点：**

首先得在本地设计一个机器学习算法，然后将其放到云上用不同的参数和数据集来训练模型。这第二步，将算法放到云上进行全面的训练，所耗费的时间要比想象的更长，通常让人很沮丧而且涉及到很多陷阱。

**Kubernetes来改善深度学习云上训练过程：**

 使用k8s和docker工具，可以轻松地使用Kubernetes上的GPU集群来自动化和加速他们的深度学习训练，从而极大地改进在云上训练模型的过程。

<center><h3>简化后的部署</h3></center>

 ![000](/img/devops/000.jpg)

**集群结构粗览**

核心想法是用一个很小的只有CPU的master节点来控制一组GPU worker节点。

<center><h3>K8s集群结构</h3></center>

![001](/img/devops/001.jpg)


### 4.2. 模型转换实现多平台部署
 通过模型转换技术将训练的算法模型转换成不同框架可用的模型，比如将Tensorflow训练的算法模型，通过CNTK部署到嵌入式设备（主要是移动设备arm架构硬件），实现移动设备的智能化。

 微软开源了 MMdnn，这是一套能让用户在不同深度学习框架间做相互操作的工具。比如，模型的转换和可视化，并且可以让模型在 Caffe、Keras、MXNet、Tensorflow、CNTK、PyTorch 和 CoreML 之间转换。除此之外，微软还推出了深度学习框架的通用语言repo 1.0以深度学习联合标准ONNX。

**相关技术介绍：**

Github 地址：https://github.com/Microsoft/MMdnn

开放式神经网络交换（ONNX）https://github.com/onnx/onnx

MMdnn 中的「MM」代表模型管理，「dnn」的意思是深度神经网络。它可以将由一个框架训练的 DNN 模型转换到其他框架里，其主要的特点如下：

* Model File Converter 在不同框架间转换 DNN 模型。

* Model Code Snippet Generator 为框架生成训练代码

* Model Visualization DNN 网络结构和框架参数可视化

* Model compatibility testing（正在开发中）

![e474de838e1e4677a53b177e75393b70](/img/devops/e474de838e1e4677a53b177e75393b70.jpeg)

Repo 1.0目前主要的工作是构建一个跨平台、跨架构、跨硬件的基准测试环境，让开发者和研究人员根据自己的需求，选择最恰当的平台、硬件和深度学习框架。

**Repo 1.0完整版GitHub链接：** https://github.com/ilkarman/DeepLearningFrameworks

![29767d14298a4a778ab8710bc6fb6c29](/img/devops/29767d14298a4a778ab8710bc6fb6c29_u51gravkm.jpeg)


**测试MMdnn转换模型**

MMdnn的github仓库上展示了，部分ImageNet模型上对当前支持的框架间模型转换功能进行了测试。

![20180529213613](/img/devops/20180529213613.png)

**模型可视化**

你可以使用 MMdnn 模型可视化工具（http://vis.mmdnn.com/），提交自己的 IR json 文件进行模型可视化。为了运行下面的命令行，你需要使用喜欢的包管理器安装 requests、Keras、TensorFlow。

![vismmdnn](/img/devops/vismmdnn.png)

## 5. DeveOps分享文化建设
### 5.1.	打造分享文化
 创建内部分享组织Sharemix（诞生和介绍：http://wiki.nat.xinktech.com/books/Sharemix-2018 ）。

**引言**
 三人行必有我师，所以提议大家互相学习，每周大家把自己所学进行团队分享，以前是自己学习，同一时间只能获取一种技术知识，但是把大家的知识汇集后，进行内部学习，这样一周你所学的到的东西是 原来的n倍（打个比方：每个人只有一玩具，有10个小伙伴，怎样玩10玩具？是的，你要与别人分享才能做到。），短时间内效果不是很明显，但是长期下来，团队的技能会整体提高很多，从而形成公司的特有文化。

 **什么是Sharemix**
 Sharemix是Share 和 remix的组合，意思是将idea进行分享和再混合，杂交出更棒的灵感，让大家更具创新能力和意识，明白分享精神的重要，以及通过分享和倾听提高自己。人类的发展是因为会交流、学习、使用工具才进步的，然而实现这些能力如果没有分享和传承，我想人类是无法进化和科技进步的，Github之所以火是因为提供了分享代码和交流的平台，帮geek们搭建了一个地方，而我们的Sharemix就像你当初学英语的英语角，一群志同道合的伙伴进行分享和学习共同进步，我们每一个人都是Sharemixer。

 ![1520a062a00a79ff](/img/devops/1520a062a00a79ff.png)

 **Sharemix诞生**
 Sharemix诞生于2018年3月30日 ，每一个参与Sharemix的人我们称之为Sharmixer（ 谐音可以理解为 Share remix sir ！哈哈哈）分享再混合是产生创造力的源泉，话说不会裁缝的厨师不是好司机，科技、互联网时代讲究的就是混搭，人工智能领域更是各种科技领域的混搭，如果不混搭不remix 就会被fixed！

**Sharemix目的**

此次提议的目的是通过知识分享扩展每个人的知识面和技术水平，同时提高每个人的沟通、表达以及演讲能力，20分钟是人能集中注意力的最佳时间段，太短或太长效果都不好，所以20分钟讲明白一个事情，不但对演讲者有要求对听的人也有要求，因为你得边听边思考，并且准备问题，最后会有5分钟的Q/A环节，前三提的具有建设性问题的同事给予小礼品奖励表示鼓励，往往灵感和创新来源于此，这就是头脑风暴最好的方式！

**Sharemix的价值**
俗话说一人难抵千军万马，所以一个人的力量再强也无法和一个团队进行对抗。由于团队成员的水平参差不齐，所以最短版的那个决定了团队的整体能力，这与木桶效应是一样的。所以Sharemix存在的价值就是取长补短，去粗取精！Sharemix有机会让每个人成为专业领域的老师帮助他人和自己提高，从而达到提升团度整体水平的能力。Sharermix为分享而生，它将保存和传承公司的技术精华，为企业建设自己的知识库和社区组织，这就是它存在的价值。

**Sharemix的使命**

Sharemix的使命是将分享精神撒到公司的各个地方，如果上天给Sharemix一次机会，Sharemix想成为互联网上的一个标志，如果非得价格期限，我希望是一万年！

**文档存放方式和要求**

1、 采用DocStack进行管理，方便分享、编辑和管理；
2、 以Sharemix开头用 - 隔开表示链接，比如Sharemix-2018-3-30-Docker分享；
3、 创建文档的时候尽可能的简明扼要，层次分明，思路清晰；

### 5.2.	Sharemix分享流程和实施方案
 定期准备分享清单，根据大家的投票确定分享主题和Sharemixer。

 1、先通过问卷获取大家的兴趣方向
 2、根据调查结果通过与Sharemixer的沟通确定分享的
 https://wj.qq.com/s/1973144/ed13

 分享时间定于每周五北京时间下午16点整，为期1-2h，每人10-20分钟的分享时间，每周大概可以让3-5人的分享。16点准时开始分享活动，每周星期一到星期四报名 ，期待大家踊跃报名和自荐 ！主题和内容参照上图的调查，好的主题或者公司必用技能的可以进行多阶段分多次分享，名额有限先报先得！分享的Doc可以用我们的DocStack进行记录（可以提前准备好，也可以事后补充后分享），因为每个文档都是sharer的辛勤劳动成果，为了营造更好的气氛，吸引更多的分享者和观众，DocStack支持在线打赏（上传微信或支付宝的二维码即可），当然根据评论和评分就能体现该分享的影响力和口碑，这是一个双赢的机会，希望分享者踊跃报名！

### 5.3.	内部技术文档管理方案
 用于企业内部的DocStack可以满足公司内部文档和技术文档分享，并已经开源，后期会继续维护。

**DocStackS（多克）的诞生**

![book](/img/devops/book.png)

公司内部定期会组织分享和培训，经常会有会议笔记和相关文档，虽然有很多云笔记可以用
，但是企业的资料还是希望能够自己掌控，因此DocStack诞生于公司内部的文档管理需求。DocStack用于知识分享和笔记，DocStack是基于BookStack开发的，为文档管理和知识分享而生。

多克(DocStack.top) 仅提供文档编写、整理、归类等功能，以及对文档内容的生成和导出工具。

分享，让知识传承更久远！ 感谢知识的创造者，感谢知识的分享者，也感谢每一位阅读到此处的
读者，因为我们都将成为知识的传承者。

**开源地址：** https://github.com/JermineHu/DocStack

### 5.4.	文件分享方案
 文件分享方案采用Nextcloud。

 相对简单靠谱的方案有购买 Office 365 用微软的 OneDrive、或使用国外其他的 Dropbox、Google Drive 等、或者购买 NAS 设备。当然，你还可以用自家/公司的电脑或租用 VPS 服务器来「搭建自己的私有云网盘」！除了 SeaFile、ownCloud、Daemon Sync 外，新一代的开源网盘 Nextcloud 同样值得推荐。

**Nextcloud - 打造私有云网盘**

 Nextcloud 是一个免费专业的私有云存储网盘「开源」项目，可以让你简单快速地在个人/公司电脑、服务器甚至是树莓派等设备上架设一套属于自己或团队专属的云同步网盘，从而实现跨平台跨设备文件同步、共享、版本控制、团队协作等功能。Nextcloud，可以看作是 ownCloud 的下一代后续版本，它修复了更多的 bug 、支持更多的平台、并且也加入很多 ownCloud 没有的新特性，而且依然保持开源免费，所以考虑到后续的维护升级，选它是个不错的主意。

![20180529215322](/img/devops/20180529215322.png)

Nextcloud 跨平台支持 Windows、Mac、Android、iOS、Linux 等平台，而且还提供了「网页版」以及 WebDAV 形式访问，因此你几乎可以在任何电脑、手机设备上都能轻松获取和访问你的文件文档。

另外，Nextcloud 还支持 API 和插件扩展，用户可以通过安装各种「插件」来增强网盘的功能，比如 Markdown 编辑器、笔记、日历、任务列表、音乐播放器、文档编辑等等,总体来说NextCloud很适合企业内文件分享的功能。
