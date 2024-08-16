---
title: "UUID"
date: 2024-06-12
description: ""
cover: https://github.com/Gjt-9520/MarkDownBlog/blob/main/source/coverImages/Bimage-135/Bimage44.jpg?raw=true
tags: ["UUID"]
category: "笔记"
updated: 2024-06-13
  
top_group_index: 
---

# UUID

UUID(通用唯一标识符)是一种用于标识信息的标准格式

它是由128位二进制数所表示的,通常以32位的16进制数显示

形如`xxxxxxxx-xxxx-Mxxx-Nxxx-xxxxxxxxxxxx`

其中每个x是一个16进制数字(0-9或a-f),M表示UUID的版本号,N表示UUID的变体

UUID的生成通常基于不同的算法,例如基于时间戳、随机数等

# 特点

1. 全球唯一性:根据其设计,UUID应该在空间和时间上都是唯一的,即使是在分布式系统中也是如此
2. 无序性:UUID是随机生成的,没有按照任何特定的顺序排列
3. 固定长度:UUID是128位长,无论是作为十六进制数还是字符串表示,其长度都是固定的
4. 易于生成:由于其算法的设计,UUID 的生成相对容易且高效

# 作用

UUID在许多领域都有广泛的应用,例如数据库主键、分布式系统中的节点标识、会话标识、文件命名等等

由于其全球唯一性和随机性,UUID在避免命名冲突和提高系统可伸缩性方面非常有用

# 使用

`String str = UUID.randomUUID().toString();`:常规的uuid

`String str = UUID.randomUUID().toString().replace("-", "");`:去除中间的`-`的uuid