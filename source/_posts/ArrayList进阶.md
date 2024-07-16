---
title: "ArrayList进阶"
date: 2024-03-14
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage72.jpg?raw=true
tags: ["JavaSE","集合"]
category: "笔记"
updated: 2024-03-15

top_group_index:
---

# ArrayList底层原理

ArrayList: `Array` -- 数组,List -- 属于List系列的一员            

ArrayList底层数据结构是数组(`Object[] elementData`)

# 特点

1. **有序**: 存和取的元素顺序一致
2. **有索引**: 可以通过索引操作元素
3. **可重复**: 存储的元素可以重复

# 底层原理

![ArrayList底层原理1](../images/ArrayList底层原理1.png)

![ArrayList底层原理2](../images/ArrayList底层原理2.png)

![ArrayList底层原理3](../images/ArrayList底层原理3.png)

![ArrayList底层原理4](../images/ArrayList底层原理4.png)

![ArrayList底层原理5](../images/ArrayList底层原理5.png)

# ArrayList底层源码分析

![ArrayList底层原理源码1](../images/ArrayList底层原理源码1.png)

![ArrayList底层原理源码2](../images/ArrayList底层原理源码2.png)