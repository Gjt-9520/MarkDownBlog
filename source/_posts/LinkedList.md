---
title: "LinkedList"
date: 2024-03-15
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage91.jpg?raw=true
tags: ["Java SE","集合"]
category: "学习笔记"
updated: 2024-03-16

top_group_index:
---

# LinkedList

LinkedList: `Linked` -- 链表,List -- 属于List系列的一员

![双向链表](../images/双向链表.png)

LinkedList本身多了很多直接操作首尾元素的特有方法

# 特点

**查询慢,增删快**(如果操作是首位元素,速度也是快的)   

1. **有序**: 存和取的元素顺序一致
2. **有索引**: 可以通过索引操作元素
3. **可重复**: 存储的元素可以重复

# 特有方法

![LinkedList特有API](../images/LinkedList特有API.png)

# LinkedList底层原理

LinkedList底层数据结构是双链表

# LinkedList底层源码分析

![LinkedList底层源码分析1](../images/LinkedList底层源码分析1.png)

![LinkedList底层源码分析2](../images/LinkedList底层源码分析2.png)

![LinkedList底层源码分析3](../images/LinkedList底层源码分析3.png)