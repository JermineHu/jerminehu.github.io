---
title: "使用dotnet core通过docker部署一个基于微服务的demo应用"
date: 2018-11-27T13:25:20+08:00
categories: ["All","dotnet core","docker","microservice","MQ","kubernetes"]
tags: ["dotnet core","docker","microservice","MQ","kubernetes"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["dotnet core","docker","microservice","MQ","kubernetes"]
description: "使用dotnet core通过docker部署一个基于微服务的demo应用"
---


## 使用dotcore通过docker部署一个基于微服务的demo应用

### 架构概述

这个参考应用程序在服务器和客户端是跨平台的，这要归功于.NET Core服务能够在Linux或Windows容器上运行，具体取决于您的Docker主机，以及Xamarin用于在Android，iOS或Windows / UWP plus上运行的移动应用程序客户端Web应用程序的任何浏览器。该体系结构提出了一种面向微服务的体系结构实现，具有多个自治微服务（每个都拥有自己的数据库/ db），并在每个微服务中实现不同的方法（简单的CRUD与DDD / CQRS模式），使用Http作为客户端应用程序之间的通信协议。微服务和支持异步通信，用于基于集成事件和事件总线（轻量级消息代理，在RabbitMQ或Azure Service Bus之间进行选择，跨多个服务）进行数据更新传播。

### 架构图如下

![１](/img/microservice/.net/1.png)


### 项目地址

https://github.com/dotnet-architecture/eShopOnContainers