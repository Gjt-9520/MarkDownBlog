---
title: "LinkedList"
date: 2024-03-15
description: "特点、特有方法、底层原理、底层源码分析"
cover: https://github.com/Gjt-9520/ImageResources/blob/main/photos/original/Ximage91.jpg?raw=true
tags: ["Java进阶","集合"]
category: "学习笔记"
updated: 2024-03-15
---

## LinkedList

`LinkedList`: `Linked` -- 链表,`List` -- 属于`List`系列的一员

![双向链表](../images/双向链表.png)

`LinkedList`本身多了很多直接操作首尾元素的特有方法

## 特点

**查询慢,增删快**(如果操作是首位元素,速度也是快的)   

## 特有方法

![LinkedList特有API](../images/LinkedList特有API.png)

## LinkedList底层原理

`LinkedList`底层数据结构是双链表

## LinkedList底层源码分析

![LinkedList底层源码分析1](../images/LinkedList底层源码分析1.png)

![LinkedList底层源码分析2](../images/LinkedList底层源码分析2.png)

![LinkedList底层源码分析3](../images/LinkedList底层源码分析3.png)