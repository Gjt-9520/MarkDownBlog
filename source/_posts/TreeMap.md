---
title: "TreeMap"
date: 2024-03-24
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage107.jpg?raw=true
tags: ["JavaSE","集合"]
category: "学习笔记"
updated: 2024-03-25
swiper_index:
top_group_index:
---

# 特点

特点都是**由键决定的:可排序**、不重复、无索引                      
这里的可排序指的是对键进行排序                         
默认是按照键的从小到大进行排序,也可以规定键的排序规则                  

## 底层原理

基于**红黑树**的数据结构实现排序的,增删改查性能都较好

## 排序规则

1. 对于数值类型:Integer、Double默认按照从小到大的顺序进行排序
2. 对于字符、字符串类型:按照字符在ASCII码表中的数字升序进行排序

# 两种比较方式

**使用原则:默认使用第一种,如果第一种不能满足当前需求,就使用第二种**

**如果两种比较方式都存在,系统会优先调用第二种比较方式**

## 默认排序/自然排序

JavaBean类实现Comparable接口指定比较规则

`return this.getAge() - o.getAge();`                 
其中this表示要添加的元素,o表示已经在红黑树中存在的元素

返回值:
1. 负数:认为要添加的元素是小的,存左边
2. 正数:认为要添加的元素是大的,存右边
3. 0:认为要添加的元素已经存在,舍弃

## 比较器排序

创建TreeSet对象的时候,传递比较器Comparetor指定规则

练习:

键:整数表示id              
值:字符串表示商品名称                 
要求:按照id的升序排列,按照id的降序排列

```java
import java.util.Comparator;
import java.util.LinkedHashMap;
import java.util.TreeMap;
import java.util.function.BiConsumer;

public class Test {
    public static void main(String[] args) {
        // 默认是升序,不传递比较器Comparator指定规则也是升序
        TreeMap<Integer, String> map1 = new TreeMap<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2;
            }
        });
        map1.put(1, "冬瓜");
        map1.put(2, "西瓜");
        map1.put(3, "黄瓜");
        map1.put(4, "苦瓜");
        map1.forEach(new BiConsumer<Integer, String>() {
            @Override
            public void accept(Integer integer, String s) {
                // 打印结果:"1,冬瓜"
                // 打印结果:"2,西瓜"
                // 打印结果:"3,黄瓜"
                // 打印结果:"4,苦瓜"
                System.out.println(integer + "," + s);
            }
        });
        System.out.println();

        TreeMap<Integer, String> map2 = new TreeMap<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });
        map2.put(1, "冬瓜");
        map2.put(2, "西瓜");
        map2.put(3, "黄瓜");
        map2.put(4, "苦瓜");
        map2.forEach(new BiConsumer<Integer, String>() {
            @Override
            public void accept(Integer integer, String s) {
                // 打印结果:"4,苦瓜"
                // 打印结果:"3,黄瓜"
                // 打印结果:"2,西瓜"
                // 打印结果:"1,冬瓜"
                System.out.println(integer + "," + s);
            }
        });
    }
}
```

练习:

键:学生对象
值:籍贯
要求:按照学生年龄的升序排列,年龄一样按照姓名的字母排列,同姓名年龄视为同一个人

```java
import java.util.Objects;

public class Student {
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

```java
import java.util.*;
import java.util.function.BiConsumer;

public class StudentTest {
    public static void main(String[] args) {
        Student s1 = new Student("zhansan", 23);
        Student s2 = new Student("lisi", 24);
        Student s3 = new Student("wangwu", 24);
        Student s4 = new Student("wangwu", 24);
        TreeMap<Student, String> map = new TreeMap<>(new Comparator<Student>() {
            @Override
            public int compare(Student o1, Student o2) {
                int ageSort = o1.getAge() - o2.getAge();
                return ageSort == 0 ? o1.getName().compareTo(o2.getName()) : ageSort;
            }
        });
        map.put(s1, "北京");
        map.put(s2, "上海");
        map.put(s3, "广州");
        map.put(s4, "深圳");

        // Lambda表达式
        map.forEach((student, value) ->
                // 打印结果:"姓名:zhansan,年龄:23,籍贯:北京"
                // 打印结果:"姓名:lisi,年龄:24,籍贯:上海"
                // 打印结果:"姓名:wangwu,年龄:24,籍贯:深圳"
                System.out.println("姓名:" + student.getName() + ",年龄:" + student.getAge() + ",籍贯:" + value));
    }
}
```

练习:

需求:字符串"aababcabcdabcde"                
请统计字符串中每一个字符出现的次数,并按照以下的格式输出                  
输出字符串:"a(5)b(4)c(3)d(2)e(1)"

```java
import java.util.ArrayList;
import java.util.StringJoiner;
import java.util.TreeMap;

public class Test {
    public static void main(String[] args) {
        String str = "aababcabcdabcde";
        TreeMap<Character, Integer> map = new TreeMap<>();
        for (int i = 0; i < str.length(); i++) {
            char c = str.charAt(i);
            // 查看字符是否已经添加到集合当中
            if (map.containsKey(c)) {
                // 如果已添加,统计其字符已经出现的次数
                Integer count = map.get(c);
                // 字符又出现了一次
                count++;
                // 将当前字符的出现次数再次存入map集合中
                map.put(c, count);
            } else {
                // 如果未添加,将其添加到map集合中
                map.put(c, 1);
            }
        }

//        // 利用StringJoiner进行拼接
//        StringJoiner sb1 = new StringJoiner("", "", "");
//        // 遍历map集合并打印
//        map.forEach((character, integer) ->
//                // 按照指定格式进行拼接
//                sb1.add(character + "").add("(").add(integer + "").add(")")
//        );
//        // 打印结果:"a(5)b(4)c(3)d(2)e(1)"
//        System.out.println(sb1);

        // 利用StringBuilder进行拼接
        StringBuilder sb2 = new StringBuilder();
        // 遍历map集合并打印
        map.forEach((character, integer) ->
                // 按照指定格式进行拼接
                sb2.append(character).append("(").append(integer).append(")")
        );
        // 打印结果:"a(5)b(4)c(3)d(2)e(1)"
        System.out.println(sb2);
    }
}
```

# 底层原理

