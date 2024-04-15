---
title: "HashSet"
date: 2024-03-18
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage81.jpg?raw=true
tags: ["Java进阶","集合"]
category: "学习笔记"
updated: 2024-03-19
swiper_index:
top_group_index:
---

## HashSet

HashSet: `Hash` -- 哈希表,`Set` -- 属于`Set`系列的一员

HashSet集合底层采取**哈希表**存储数据

### 哈希表

哈希表是一种对于增删改查数据性能都较好的结构

哈希表组成:
1. JDK8之前:数组+链表
2. JDK8之后:数组+链表+红黑树

#### 哈希值

哈希值:对象的整数表现形式

- 根据hashCode方法算出来的int类型的整数
- 该方法定义在Object类中,所有的对象都可以调用,默认使用地址值进行计算
- 一般情况下,会重写hashCode方法,利用对象内部的属性值计算哈希值

特点:
1. 如果没有重写hashCode方法,不同对象计算出的哈希值是不同的
2. 如果已经重写hashCode方法,不同对象只要属性值相同,其计算出的哈希值是一样的
3. 在小部分情况下,不同的属性值或者不同的地址值计算出来的哈希值也有可能是一样的(哈希碰撞)

