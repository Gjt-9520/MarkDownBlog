---
title: "HashMap"
date: 2024-03-22
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage105.jpg?raw=true
tags: ["JavaSE","集合"]
category: "学习笔记"
updated: 2024-03-23
swiper_index:
top_group_index:
---

# 特点 

1. HashMap是Map里面的一个实现类
2. 没有额外需要学习的特有方法,直接使用Map里面的方法即可
3. 特点都是**由键决定的:无序、不重复、无索引**
4. HashMap跟HashSet底层原理是一模一样的,都是哈希表的数据结构               
但是HashMap是**利用键计算哈希值,跟值无关**,equals()方法比较的也是**键**                
即依赖hashCode和equals方法保证**键的唯一**                 

![HashMap哈希表底层原理](../images/HashMap哈希表底层原理.png)

5. 如果**键**存储的是自定义对象,**需要重写**hashCode和equals方法                 
如果**值**存储的是自定义对象,**不需要重写**hashCode和equals方法    

练习:

创建一个HashMap集合,键是学生对象(Student),值是籍贯(String)                    
存储四个键值对元素并遍历                    
要求:同姓名、同年龄认为是同一个学生            

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

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student student = (Student) o;
        return age == student.age && Objects.equals(name, student.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
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
        Student s3 = new Student("wangwu", 25);
        Student s4 = new Student("wangwu", 25);
        HashMap<Student, String> map = new HashMap<>();
        map.put(s1, "北京");
        map.put(s2, "上海");
        map.put(s3, "广州");
        map.put(s4, "深圳");

        // 键找值
        Set<Student> set = map.keySet();
        for (Student student : set) {
            String value = map.get(student);
            // 打印结果:"姓名:zhansan,年龄:23,籍贯:北京"
            // 打印结果:"姓名:wangwu,年龄:25,籍贯:深圳"(深圳覆盖广州)
            // 打印结果:"姓名:lisi,年龄:24,籍贯:上海"
            System.out.println("姓名:" + student.getName() + ",年龄:" + student.getAge() + ",籍贯:" + value);
        }
        System.out.println();

        // 键值对
        Set<Map.Entry<Student, String>> entries = map.entrySet();
        for (Map.Entry<Student, String> entry : entries) {
            String name = entry.getKey().getName();
            int age = entry.getKey().getAge();
            System.out.println("姓名:" + name + ",年龄:" + age + ",籍贯:" + entry.getValue());
        }
        System.out.println();

        // Lambda表达式
        map.forEach((student, value) ->
                System.out.println("姓名:" + student.getName() + ",年龄:" + student.getAge() + ",籍贯:" + value));
    }
}
```

练习:

某个班级8名学生,现在需要组成秋游活动,班长提供了四个景点,依次是(A,B,C,D)                 
每个学生只能选择一个景点,请统计出最终哪个景点想去的人数最多      

```java
import java.util.Objects;

public class Student {
    private String name;

    public Student() {
    }

    public Student(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student student = (Student) o;
        return Objects.equals(name, student.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name);
    }
}
```

```java
import java.util.HashMap;
import java.util.Random;
import java.util.function.BiConsumer;

public class StudentTest {
    static int ACount = 0;
    static int BCount = 0;
    static int CCount = 0;
    static int DCount = 0;

    public static void main(String[] args) {
        Student s1 = new Student("zhangsan");
        Student s2 = new Student("lisi");
        Student s3 = new Student("wangwu");
        Student s4 = new Student("zhaoliu");
        Student s5 = new Student("tianqi");
        Student s6 = new Student("xiaowang");
        Student s7 = new Student("xiaogu");
        Student s8 = new Student("xiaozhang");
        HashMap<Student, String> map = new HashMap<>();
        map.put(s1, randomSight());
        map.put(s2, randomSight());
        map.put(s3, randomSight());
        map.put(s4, randomSight());
        map.put(s5, randomSight());
        map.put(s6, randomSight());
        map.put(s7, randomSight());
        map.put(s8, randomSight());
        map.forEach(new BiConsumer<Student, String>() {
            @Override
            public void accept(Student student, String sight) {
                switch (sight) {
                    case "A":
                        ACount++;
                        break;
                    case "B":
                        BCount++;
                        break;
                    case "C":
                        CCount++;
                        break;
                    default:
                        DCount++;
                        break;
                }
                System.out.println(student.getName() + "想去" + sight);
            }
        });
        maxSight(ACount, BCount, CCount, DCount);
    }

    public static void maxSight(int ACount, int BCount, int CCount, int DCount) {
        int maxCount = Math.max(Math.max(ACount, BCount), Math.max(CCount, DCount));
        if (maxCount == ACount) {
            System.out.println("想去A景点的人数最多");
        } else if (maxCount == BCount) {
            System.out.println("想去B景点的人数最多");
        } else if (maxCount == CCount) {
            System.out.println("想去C景点的人数最多");
        } else {
            System.out.println("想去D景点的人数最多");
        }
    }

    public static String randomSight() {
        Random r = new Random();
        String[] arr = new String[]{"A", "B", "C", "D"};
        return arr[r.nextInt(4)];
    }
}
```

练习:

某个班级80名学生,现在需要组成秋游活动,班长提供了四个景点,依次是(A,B,C,D)                 
每个学生只能选择一个景点,请统计出最终哪个景点想去的人数最多          

```java
import java.util.*;

public class sightChoose {
    public static void main(String[] args) {
        // 定义arr数组,存储四个景点
        String[] arr = {"A", "B", "C", "D"};
        // 定义list集合,存储投票的结果
        ArrayList<String> list = new ArrayList<>();
        // 随机生成80个投票结果,存入集合中
        Random r = new Random();
        for (int i = 0; i < 80; i++) {
            list.add(arr[r.nextInt(4)]);
        }
        // 定义map集合进行统计
        HashMap<String, Integer> map = new HashMap<>();
        for (String sight : list) {
            // 判断当前景点是否在map集合中已经存在
            if (map.containsKey(sight)) {
                // 已经存在
                // 获取当前景点已经被投票的次数
                Integer count = map.get(sight);
                // 当前景点又被投票一次
                count++;
                // 将当前景点的投票次数再次存入map集合中
                map.put(sight, count);
            } else {
                // 不存在
                map.put(sight, 1);
            }
        }
        printMap(map, getMax(map));
    }

    // 获取票数中的最大值
    public static int getMax(HashMap<String, Integer> map) {
        int max = 0;
        Set<Map.Entry<String, Integer>> entries = map.entrySet();
        for (Map.Entry<String, Integer> entry : entries) {
            int vote = entry.getValue();
            if (vote > max) {
                max = vote;
            }
        }
        return max;
    }

    // 根据票数最大值找到对应的景点,并打印map集合
    public static void printMap(HashMap<String, Integer> map, int maxVote) {
        map.forEach((sight, vote) ->
                System.out.println("景点" + sight + "的票数为" + vote)
        );
        System.out.print("想去人数最多的景点是:");
        map.forEach((sight, vote) -> {
            if (maxVote == vote) {
                System.out.print(sight + " ");
            }
        });
    }
}
```

# 底层原理

