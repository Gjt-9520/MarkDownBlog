---
title: "int与String之间的转换"
date: 2023-12-31
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage83.jpg?raw=true
tags: ["类型转换"]
category: "学习笔记"
updated: 2024-01-01
swiper_index:
top_group_index:
---

# int --> String

`int、double、float、Long --> String`的方法大同小异    

## 方法1

`String str = String.valueOf(i);`    
直接使用String类的静态方法,只产生一个对象   

范例: 

```java
public class test {
    public static void main(String[] args) {
        int i = 123;
        String str = String.valueOf(i);
        // 打印结果:"123"
        System.out.println(str);
    }
}
```

## 方法2

`String str = Integer.toString(i);`   
toString方法的底层是直接new了一个字符串对象    

范例: 

```java
public class test {
    public static void main(String[] args) {
        int i = 123;
        String str = Integer.toString(i);
        // 打印结果:"123"
        System.out.println(str);
    }
}
```

`String str = Integer.toString(i,n);`   
其中i表示转成字符串的数字,n表示要转成的进制表示    

范例: 

```java
public class test {
    public static void main(String[] args) {
        int i = 123;
        // 将十进制数123转换为用2进制表示的字符串1111011
        String str = Integer.toString(i,2);
        // 打印结果:"1111011"
        System.out.println(str);
    }
}
```

## 方法3

`String str = "" + i;`    
会产生两个String对象    

范例: 

```java
public class test {
    public static void main(String[] args) {
        int[] arr = {1,2,3};
        String str = "";
        for (int i = 0; i < arr.length; i++) {
            str += arr[i];
        }
        // 打印结果:"123"
        System.out.println(str);
    }
}
```

# String --> int

`String --> int、double、float、Long`的方法大同小异    

## 方法1

`int i = Integer.parseInt(str);`    
其中Integer.parseInt(str)返回原始int   
直接使用静态方法,不会产生多余的对象    

范例: 

```java
public class test {
    public static void main(String[] args) {
        String str = "123";
        int i = Integer.parseInt(str);
        // 打印结果:"123"
        System.out.println(i);
    }
}
```

`int i = Integer.parseInt(str,n);`    
其中str表示转成数字的字符串,n表示要转成的进制表示   

范例: 

```java
public class test {
    public static void main(String[] args) {
        String str = "11100111";
        // 将二进制数11100111转换成十进制数字231
        int i = Integer.parseInt(str,2);
        // 打印结果:"231"
        System.out.println(i);
    }
}
```

## 方法2

`int i = Integer.valueOf(str);`    
其中Integer.valueOf(str)返回一个Integer对象   

范例: 

```java
public class test {
    public static void main(String[] args) {
        String str = "123";
        int i = Integer.valueOf(str);
        // 打印结果:"123"
        System.out.println(i);
    }
}
```