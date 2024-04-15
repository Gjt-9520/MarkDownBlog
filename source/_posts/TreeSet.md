---
title: "TreeSet"
date: 2024-03-20
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage100.jpg?raw=true
tags: ["Java进阶","集合"]
category: "学习笔记"
updated: 2024-03-21
swiper_index:
top_group_index:
---

# TreeSet

TreeSet: `Tree` -- 树,`Set` -- 属于`Set`系列的一员

# 特点

1. **可排序**:按照元素的默认规则(从小到大)排序
2. 不重复:可以去除重复
3. 无索引:没有带索引的方法,所以不能使用普通的for循环遍历,也不能通过索引来获取元素

# 底层原理

基于红黑树的数据结构实现排序的,增删改查性能都较好

练习: 

存储整数并进行排序

```java
import java.util.TreeSet;

public class test {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(12);
        set.add(123);
        set.add(21);
        set.add(11);
        for (Integer integer : set) {
            // 打印结果:"11 12 21 123 "
            System.out.print(integer + " ");
        }
    }
}
```