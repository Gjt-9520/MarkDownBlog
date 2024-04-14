---
title: "final"
date: 2024-01-08
description: "final修饰的方法、类、变量"
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage79.jpg?raw=true
tags: ["Java基础","面向对象"]
category: "学习笔记"
updated: 2024-01-09
swiper_index:
top_group_index:
---

## 包

包就是文件夹,用来管理各种不同功能的Java类,方便后期代码维护

### 包名的规则

`公司域名反写+包的作用`,需要全部英文小写,见名知意   

### 使用其他类的规则

1. 使用同一个包中的类时,不需要导包  
2. 使用java.lang包中的类时,不需要导包
3. 其他情况都需要导包
4. 如果同时使用两个包中的同名类,需要用全类名(这样就不需要导包)
全类名(全限定名): `包名+类名`

范例: 

Domain1包中的Teacher类: 

```java
package Domain1;

public class Teacher {
    private String name;
    private int age;

    public Teacher() {
    }

    public Teacher(String name,int age) {
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
}
```

Domain2包中的Teacher类: 

```java
package Domain2;

public class Teacher {
    private String name;
    private int age;

    public Teacher() {
    }

    public Teacher(String name,int age) {
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
}
```

测试包中的Student类: 

```java
package Test;

public class Student {
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name,int age) {
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
}
```

测试包中的Test类: 

```java
package Test;

import Domain1.Teacher;

public class Test {
    public static void main(String[] args) {
        //使用同一个包中的类时,不需要导包
        Student student = new Student("张三",23);
        System.out.println(student.getName() + "," + student.getAge());

        //使用java.lang包中的类时,不需要导包
        String str = "abc";
        System.out.println(str);

        //使用其他包中类,需要导包
        Teacher teacher = new Teacher("赵老师",20);
        System.out.println(teacher.getName() + "," + teacher.getAge());

        //如果同时使用两个包中的同名类,需要用全类名(这样就不需要导包)
        Domain1.Teacher teacher1 = new Domain1.Teacher("李四",24);
        Domain2.Teacher teacher2 = new Domain2.Teacher("王五",25);
        System.out.println(teacher1.getName() + "," + teacher1.getAge());
        System.out.println(teacher2.getName() + "," + teacher2.getAge());
    }
}
```

## final

final即最终的,被final修饰的不可被改变

### 被final修饰的方法

表明该方法是最终方法,不能被重写

### 被final修饰的类

表明该类是最终类,不能被继承

### 被final修饰的变量

叫做常量,只能被赋值一次     
使用场景: 当一个变量的值,不想让其再发生改变了,就用final来修饰   

#### 常量

实际开发中,常量一般作为系统的配置信息,方便维护,提高可读性

##### 命名规范

1. 单个单词: 全部大写
2. 多个单词: 全部大写,单词之间用下划线隔开

##### 细节

1. **常量记录的数据是不能发生改变的**   
2. final修饰的变量是基本类型: 那么变量存储的**数据值**不能发生改变    
3. final修饰的变量是引用类型: 那么变量存储的**地址值**不能发生改变,对象内部的**属性值**可以改变   

范例: 

```java
public class Student {
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name,int age) {
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
}
```

```java
public class Test {
    public static void main(String[] args) {
        //final修饰的变量是基本类型: 那么变量存储的**数据值**不能发生改变
        final double PI = 3.1415926;
        //`PI = 2;`报错

        //final修饰的变量是引用类型: 那么变量存储的**地址值**不能发生改变,对象内部的属性值可以改变
        final Student STUDENT = new Student("张三",24);
        //`student = new Student();`报错
        STUDENT.setName("王五");
        STUDENT.setAge(25);
        //打印结果: "王五,25"
        System.out.println(STUDENT.getName() + "," + STUDENT.getAge());

        final int[] ARR = {1,2,3,4,5};
        //`ARR = new int[10];`报错
        ARR[0] = 10;
        ARR[4] = 11;
        //打印结果: "10 2 3 4 11 "
        for (int j: ARR) {
            System.out.print(j + " ");
        }
    }
}
```

练习: 

将学生管理系统中用户的操作改为常量的形式

```java
public class StudentSystem {
    private static final String ADD_STUDENT = "1";
    private static final String DELETE_STUDENT = "2";
    private static final String MODIFY_STUDENT = "3";
    private static final String QUERY_STUDENT = "4";
    private static final String BACK = "5";
    private static final String EXIT = "6";

    //学生管理系统主界面
    public static void mainMenu() {
        Scanner scanner = new Scanner(System.in);
        //定义学生集合
        ArrayList<Student> list = new ArrayList<>();
        Student stu1 = new Student("202301","张三生",29,"南京");
        Student stu2 = new Student("202302","李四维",21,"上海");
        Student stu3 = new Student("202303","王五峰",22,"深圳");
        list.add(stu1);
        list.add(stu2);
        list.add(stu3);
        mainMenu:
        while (true) {
            System.out.println("-----------------");
            System.out.println("|  学生管理系统主界面\t|");
            System.out.println("|  1: 添加学生信息\t|");
            System.out.println("|  2: 删除学生信息\t|");
            System.out.println("|  3: 修改学生信息\t|");
            System.out.println("|  4: 查询学生信息\t|");
            System.out.println("|  5: 返回登录界面\t|");
            System.out.println("|  6: 退出系统\t\t|");
            System.out.println("-----------------");
            System.out.print("请输入您的选择: ");
            String choose = scanner.next();
            switch (choose) {
                case ADD_STUDENT -> addStudent(list);
                case DELETE_STUDENT -> deleteStudent(list);
                case MODIFY_STUDENT -> modifyStudent(list);
                case QUERY_STUDENT -> queryStudent(list);
                case BACK -> {
                    break mainMenu;
                }
                case EXIT -> System.exit(0);
                default -> System.out.println("没有这个选项,请重新输入!");
            }
        }
    }
    ...
}
```