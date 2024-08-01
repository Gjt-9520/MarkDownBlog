---
title: "Redis基础"
date: 2024-08-04
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage93.jpg?raw=true
tags: ["Redis"]
category: "数据库"
updated: 2024-08-05
  
top_group_index: 
---

# 主从

## 搭建主从集群

![搭建主从集群](../images/搭建主从集群.png)

如图所示,集群中有一个master节点、两个slave(replica)节点

当通过Redis的Java客户端访问主从集群时,应该做好路由:
- 如果是写操作,应该访问master节点,master会自动将数据同步给两个slave节点
- 如果是读操作,建议访问各个slave节点,从而分担并发压力

## 主从同步原理

![主从同步原理](../images/主从同步原理.png)

### 全量同步

**全量同步:master将完整内存数据生成RDB,发送RDB到slave,后续命令则记录在repl_baklog,逐个发送给slave**

主从第一次建立连接时,会执行**全量同步**,将master节点的所有数据都拷贝给slave节点,流程:

master如何得知salve是否是第一次来同步呢?

有几个概念,可以作为判断依据:
- `Replication Id`:简称replid,是数据集的标记,replid一致则是同一数据集,每个master都有唯一的replid,slave则会继承master节点的replid
- `offset`:偏移量,随着记录在repl_baklog中的数据增多而逐渐增大,slave完成同步时也会记录当前同步的offset,如果slave的offset小于master的offset,说明slave数据落后于master,需要更新

因此slave做数据同步,必须向master声明自己的replication id 和offset,master才可以判断到底需要同步哪些数据

**master判断一个节点是否是第一次同步的依据,就是看replid是否一致**

全量同步步骤:
- slave节点请求增量同步
- master节点判断replid,发现不一致,拒绝增量同步
- master将完整内存数据生成RDB,发送RDB到slave
- slave清空本地数据,加载master的RDB
- master将RDB期间的命令记录在repl_baklog,并持续将log中的命令发送给slave
- slave执行接收到的命令,保持与master之间的同步

**什么时候执行全量同步?**
- **slave节点第一次连接master节点时**
- **slave节点断开时间太久,repl_baklog中的offset已经被覆盖时**

### 增量同步

**增量同步:slave提交自己的offset到master,master获取repl_baklog中从offset之后的命令给slave**

**除了第一次做全量同步,其它大多数时候slave与master都是做增量同步**

master怎么知道slave与自己的数据差异在哪里呢?

全量同步时的repl_baklog文件是一个固定大小的数组,只不过数组是环形,也就是说**角标到达数组末尾后,会再次从0开始读写,这样数组头部的数据就会被覆盖**

repl_baklog中会记录Redis处理过的命令及offset,包括master当前的offset,和slave已经拷贝到的offset

slave与master的offset之间的差异,就是salve需要增量拷贝的数据

**什么时候执行增量同步?**
- **slave节点断开又恢复,并且在repl_baklog中能找到offset时**

### 主从同步优化

![主从同步原理](../images/主从同步原理流程.png)

- 在master中配置repl-diskless-sync  yes启用无磁盘复制,避免全量同步时的磁盘IO
- Redis单节点上的内存占用不要太大,减少RDB导致的过多磁盘IO
- 适当提高repl_baklog的大小,发现slave宕机时尽快实现故障恢复,尽可能避免全量同步
- 限制一个master上的slave节点数量,如果实在是太多slave,则可以采用主-从-从链式结构,减少master压力

![主-从-从链式结构](../images/主从从架构.png)

## 哨兵原理


## 搭建哨兵集群

# 分片集群

## 搭建分片集群

## 散列插槽

# 数据结构

## RedisObject

## SkipList

## SortedSet

# 内存回收

## 过期key处理

## 内存淘汰策略

# 缓存

## 缓存一致性

## 缓存穿透

## 缓存雪崩

## 缓存击穿
