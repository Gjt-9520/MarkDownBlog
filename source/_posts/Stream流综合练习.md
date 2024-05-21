---
title: "Stream流综合练习"
date: 2024-04-01
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage110.jpg?raw=true
tags: ["Java SE","集合","练习"]
category: "学习笔记"
updated: 2024-04-02
 
top_group_index: 
---

# 数据过滤

定义一个集合,并添加一些整数:1,2,3,4,5,6,7,8,9,10           
过滤奇数,只留下偶数    

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.stream.Collectors;

public class Test {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>();
        Collections.addAll(list, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        List<Integer> resultList = list.stream()
                .filter(number -> number % 2 == 0)
                .collect(Collectors.toList());
        for (Integer integer : resultList) {
            // 打印结果:"2 4 6 8 10"
            System.out.print(integer + " ");
        }
    }
}
```

# 数据操作1

创建一个ArrayList集合,并添加以下字符串,字符串中前面是名字,后面是年龄        

```markdown
"zhangsan,23"         
"lisi,24"         
"wangwu,25"         
```

保留年龄大于等于24岁的人,并将结果收集到Map集合中:姓名为键,年龄为值            

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.stream.Collectors;

public class Test {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        Collections.addAll(list, "zhangsan,23", "lisi,24", "wangwu,25");
        list.stream()
                .filter(str -> Integer.parseInt(str.split(",")[1]) >= 24)
                .collect(Collectors.toMap(
                        name -> name.split(",")[0],
                        age -> age.split(",")[1]))
                .forEach((name, age) -> System.out.println(name + ":" + age));
        // 打印结果:"lisi:24"
        // 打印结果:"wangwu:25"
    }
}
```

# 数据操作2

现在有两个ArrayList集合             
第一个集合中:存储6名男演员的名字和年龄                     
第二个集合中:存储6名女演员的名字和年龄         
姓名和年龄之间用逗号隔开,比如:张三,23               

要求:
1. 男演员只要名字为3个字的前两个人
2. 女演员只要姓杨的,并且不要第一个
3. 把过滤后的男演员姓名和女演员姓名合并到一起
4. 将上一步的演员对象封装成Actor对象(演员类Actor,属性只有name,age)
5. 将所有的演员对象都保存到List集合中

```java
public class Actor {
    private String name;

    private int age;

    public Actor() {
    }

    public Actor(String name, int age) {
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
        return "Actor{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class Test {
    public static void main(String[] args) {
        ArrayList<String> maleList = new ArrayList<>();
        Collections.addAll(maleList, "张三,23", "李大炮,24", "王小五,25", "大大怪,27", "小小怪,29", "张飞,55");
        ArrayList<String> femaleList = new ArrayList<>();
        Collections.addAll(femaleList, "杨西,20", "于倩倩,23", "杨璐,23", "菲菲,25", "杨飞菲,22", "吴仟,29");
        Stream<String> maleStream = maleList.stream().filter(name -> name.split(",")[0].length() == 3).limit(2);
        Stream<String> femaleStream = femaleList.stream().skip(1).filter(name -> name.startsWith("杨"));

        // List<Actor> list = Stream.concat(maleStream, femaleStream).map(new Function<String, Actor>() {
        //     @Override
        //     public Actor apply(String s) {
        //         String name = s.split(",")[0];
        //         int age = Integer.parseInt(s.split(",")[1]);
        //         return new Actor(name, age);
        //     }
        // }).collect(Collectors.toList());

        List<Actor> list = Stream.concat(maleStream, femaleStream)
                .map(s -> new Actor(s.split(",")[0], Integer.parseInt(s.split(",")[1])))
                .collect(Collectors.toList());
        // 打印结果:"李大炮,24"
        // 打印结果:"王小五,25"
        // 打印结果:"杨璐,23"
        // 打印结果:"杨飞菲,22"
        list.forEach(actor -> System.out.println(actor.getName() + "," + actor.getAge()));
    }
}
```