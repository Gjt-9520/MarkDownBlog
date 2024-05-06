---
title: "Dom4j"
date: 2024-04-03
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage83.jpg?raw=true
tags: ["Java SE","工具包"]
category: "学习笔记"
updated: 2024-04-04
swiper_index: 
top_group_index: 
---

- [Dom4j官网](https://dom4j.github.io/)

# 常用方法                                 

`public SAXReader()`:创建Dom4j的解析器对象               
`Document read(String url)`:加载XML文件成为Document对象                
`Element getRootElement()`:获得根标签对象                 
`List<Element> elements()`:得到当前标签下所有子标签                  
`List<Element> elements(String name)`:得到当前标签下指定名字的子标签,返回集合         
`Element element(String name)`:得到当前标签下指定名字的子标签,如果有同名的,返回第一个     
`String getName()`:得到标签名字              
`String attributeValue(String name)`:通过属性名直接得到属性值               
`String elementText(子标签名)`:得到指定名称的子标签的文本            
`String getText()`:得到文本                         

范例:

```xml
<students>
    <!--第一个学生信息-->
    <student id="1">
        <name>张三</name>
        <age>23</age>
    </student>

    <!--第二个学生信息-->
    <student id="2">
        <name>李四</name>
        <age>24</age>
    </student>
</students>
```

```java
public class Student {
    private String id;
    private String name;
    private String age;

    public Student() {
    }

    public Student(String id, String name, String age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAge() {
        return age;
    }

    public void setAge(String age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "id=" + id + ", name=" + name + ", age=" + age;
    }
}
```

```java
import org.dom4j.Attribute;
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.io.File;
import java.util.ArrayList;
import java.util.List;

public class Test {
    public static void main(String[] args) throws DocumentException {
        ArrayList<Student> list = new ArrayList<>();

        // 创建解析器对象
        SAXReader saxReader = new SAXReader();

        // 利用解析器去读取xml文件,返回文档对象
        File file = new File("D:\\Project\\Test\\src\\com\\jinzhao\\test4\\person.xml");
        Document document = saxReader.read(file);

        // 获取根标签
        Element rootElement = document.getRootElement();
        // 打印结果:"students"
        System.out.println(rootElement.getName());
        System.out.println();

        // 获取根标签的子标签
        List<Element> elements = rootElement.elements();
        for (Element element : elements) {
            // 获取属性id
            Attribute id = element.attribute("id");

            // 获取标签name
            Element name = element.element("name");

            // 获取标签age
            Element age = element.element("age");

            Student student = new Student(id.getText(), name.getText(), age.getText());
            list.add(student);
        }
        // 打印结果:"id=1, name=张三, age=23"
        // 打印结果:"id=2, name=李四, age=24"
        list.forEach(System.out::println);
    }
}
```

# 练习

用户登录

```xml
<users>
    <user id="1">
        <username>张三</username>
        <password>123456</password>
        <personId>123456</personId>
        <phoneId>110</phoneId>
        <admin>true</admin>
    </user>
    <user id="2">
        <username>李四</username>
        <password>123456</password>
        <personId>1234567</personId>
        <phoneId>1100</phoneId>
        <admin>false</admin>
    </user>
</users>
```

```java
public class User {
    private String id;
    private String username;
    private String password;
    private String personId;
    private String phoneId;
    private boolean admin;

    public User() {
    }

    public User(String id, String username, String password, String personId, String phoneId, boolean admin) {
        this.id = id;
        this.username = username;
        this.password = password;
        this.personId = personId;
        this.phoneId = phoneId;
        this.admin = admin;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getPersonId() {
        return personId;
    }

    public void setPersonId(String personId) {
        this.personId = personId;
    }

    public String getPhoneId() {
        return phoneId;
    }

    public void setPhoneId(String phoneId) {
        this.phoneId = phoneId;
    }

    public boolean isAdmin() {
        return admin;
    }

    public void setAdmin(boolean admin) {
        this.admin = admin;
    }

    @Override
    public String toString() {
        return "User{" +
                "id='" + id + '\'' +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", personId='" + personId + '\'' +
                ", phoneId='" + phoneId + '\'' +
                ", admin=" + admin +
                '}';
    }
}
```

```java
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.io.File;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Test {
    static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws DocumentException {
        ArrayList<User> list = new ArrayList<>();
        File file = new File("D:\\Project\\Test\\src\\com\\jinzhao\\test5\\user.xml");
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read(file);
        Element rootElement = document.getRootElement();
        List<Element> elements = rootElement.elements("user");
        for (Element element : elements) {
            String id = element.attribute("id").getText();
            String username = element.element("username").getText();
            String password = element.element("password").getText();
            String personId = element.element("personId").getText();
            String phoneId = element.element("phoneId").getText();
            String admin = element.element("admin").getText();
            User user = new User(id, username, password, personId, phoneId, Boolean.parseBoolean(admin));
            list.add(user);
        }
        while (true) {
            login(list);
        }
    }

    // 验证密码是否正确,正确则登录成功,错误则提示密码有误
    public static boolean login(ArrayList<User> list) {
        int index = isUserNameRight(list);
        // 用户名不存在,提示先注册
        if (index == -1) {
            System.out.println("用户名不存在,请先注册!");
            return false;
        } else {
            // 用户名存在,验证密码
            System.out.print("请输入密码:");
            String password = scanner.nextLine();
            for (User user : list) {
                if (user.getPassword().equals(password)) {
                    User nowUser = list.get(index);
                    if (nowUser.isAdmin()) {
                        System.out.println("登录成功!欢迎" + nowUser.getUsername() + "管理员!");
                    } else {
                        System.out.println("登录成功!欢迎" + nowUser.getUsername() + "用户!");
                    }
                    return true;
                }
            }
        }
        System.out.println("密码输入有误,请重新登录!");
        return false;
    }

    // 验证用户名是否存在,存在则返回其索引,不存在返回-1
    public static int isUserNameRight(ArrayList<User> list) {
        System.out.println("======登录界面======");
        System.out.print("请输入用户名:");
        String username = scanner.nextLine();
        for (int i = 0; i < list.size(); i++) {
            User user = list.get(i);
            if (user.getUsername().equals(username)) {
                return i;
            }
        }
        return -1;
    }
}
```