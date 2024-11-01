---
title: "SpringCloud面试题"
date: 2024-09-12
description: ""
cover: https://github.com/Gjt-9520/MarkDownBlog/blob/main/source/coverImages/Bimage-135/Bimage128.jpg?raw=true
tags: ["SpringCloud"]
category: "面试"
updated: 2024-09-13
  
top_group_index: 
---

# 你知道哪些组件，都干嘛的？

1. Sentinel：流量控制、熔断降级、系统负载保护等多个维度保护服务的稳定性。

2. Nacos：注册中心、配置管理。

3. Openfign：服务之间的远程调用。

4. Seata：处理微服务分布式事务。

# SpringBoot与SpringCloud区别？

SpringBoot是Spring的一套快速配置脚手架，可以基于SpringBoot快速开发单个微服务。

SpringCloud是一个基于SpringBoot实现的微服务开发框架。

SpringBoot可以快速、方便集成单个个体,SpringCloud是关注全局的服务治理框架。

# 你知道有哪些注册中心？

1. ZooKeeper CP

2. Eureka AP

3. Consul CP

4. Nacos CP+AP

# 什么是CAP理论?

一个分布式系统最多只能满足CAP中的两个条件，不可能同时满足三个条件。

1. C（Consistency）：这里指的是强一致性。保证在一定时间内，集群中的各个节点会达到较强的一致性，

2. A (Avalibility)：可用性。意味着系统一直处于可用状态。个别节点的故障不会影响整个服务的运作。

3. P（Partition Tolerance）：分区容忍性。当系统出现网络分区等情况时，依然能对外提供服务。

# Nacos和Eureka的区别？

- 共同点：

1. 都支持服务注册和服务拉取。

2. 都支持服务提供者心跳方式做健康检测。

- 不同点：

1. Nacos支持服务端主动检测提供者状态：临时实例采用心跳模式，非临时实例采用主动检测状态。

2. 临时实例心跳不正常会被剔除，非临时实例则不会被剔除。

3. Nacos支持服务列表变更的消息推送模式，服务列表更新及时。

4. Nacos集群默认采用AP方式，当集群中存在非临时实例时，采用CP方式。Eureka采用AP方式。