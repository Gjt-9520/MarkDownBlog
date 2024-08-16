---
title: "LinkedHashMap"
date: 2024-03-23
description: ""
cover: https://github.com/Gjt-9520/MarkDownBlog/blob/main/source/coverImages/Aimage-135/Aimage106.jpg?raw=true
tags: ["JavaSE","集合"]
category: "笔记"
updated: 2024-03-24

top_group_index:
---

# 特点

特点都是**由键决定的:有序**、不重复、无索引                      
这里的有序指的是保证存储和取出的元素顺序一致

# 原理

底层数据结构是哈希表,但是每个键值对元素又额外多了一个双链表的机制记录存储的顺序          

![LinkedHashMap底层原理](../images/LinkedHashMap底层原理.png)

范例:

```java
import java.util.LinkedHashMap;

public class Test {
    public static void main(String[] args) {
        LinkedHashMap<String, Integer> map = new LinkedHashMap<>();
        map.put("a", 123);
        map.put("a", 111);
        map.put("b", 456);
        map.put("c", 789);
        // 打印结果:"{a=111, b=456, c=789}"
        System.out.println(map);
    }
}
```