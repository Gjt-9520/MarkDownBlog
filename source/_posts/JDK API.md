---
title: "JDK API"
date: 2023-12-28
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage87.jpg?raw=true
tags: ["JavaSE","API"]
category: "学习笔记"
updated: 2023-12-29
swiper_index:
top_group_index:
---

# [JDK API 官方文档链接](https:// www.oracle.com/cn/java/technologies/java-se-api-doc.html)

# API

Application Programming Interface即应用程序编程接口

练习:

学习Scanner类的使用,实现键盘录入小数并打印在控制台

```java
import java.util.Scanner;

public class test {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入一个小数:");
        double number = sc.nextDouble();
        System.out.println("打印结果:");
        System.out.println(number);
    }
}
```