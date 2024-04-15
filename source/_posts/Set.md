---
title: "Set"
date: 2024-03-17
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage103.jpg?raw=true
tags: ["Java进阶","集合"]
category: "学习笔记"
updated: 2024-03-18
swiper_index:
top_group_index:
---

# 特点

1. 无序:存取顺序不一致
2. 不重复:可以去除重复
3. 无索引:没有带索引的方法,所以不能使用普通的for循环遍历,也不能通过索引来获取元素

范例:

```java
import java.util.HashSet;
import java.util.Set;

public class Test {
    public static void main(String[] args) throws Exception {
        Set<String> s = new HashSet<>();
        
        // 添加元素(不重复)
        // 如果当前是元素是第一次添加,那么添加成功,返回true
        // 如果当前是元素不是第一次添加,那么添加失败,返回false
        boolean r1 = s.add("张三");
        boolean r2 = s.add("张三");
        // 打印结果: "[张三]"
        System.out.println(s);
        // 打印结果: "true"
        System.out.println(r1);
        // 打印结果: "false"
        System.out.println(r2);

        // 打印集合(无序)
        s.add("李四");
        s.add("王五");
        s.add("赵六");
        // 打印结果: "[张三, 王五, 赵六]"
        System.out.println(s);
    }
}
```

# 实现类

- HashSet:**无序**、不重复、无索引
- LinkedHashSet:**有序**、不重复、无索引
- TreeSet:**可排序**、不重复、无索引

Collection的方法Set都继承了

练习:

利用Set系列的集合,添加字符串,并使用多种方式遍历

```java
import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class Test {
    public static void main(String[] args) throws Exception {
        Set<String> s = new HashSet<>();
        s.add("张三");
        s.add("李四");
        s.add("王五");
        s.add("赵六");

        // 迭代器遍历
        Iterator<String> it = s.iterator();
        while (it.hasNext()) {
            System.out.print(it.next() + " ");
        }
        System.out.println();

        // 增强for
        for (String s2 : s) {
            System.out.print(s2 + " ");
        }
        System.out.println();

        // Lambda表达式
        s.forEach(str -> System.out.print(str + " "));
    }
}
```