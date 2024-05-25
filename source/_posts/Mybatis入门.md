---
title: "Mybatis入门"
date: 2024-06-06
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage39.jpg?raw=true
tags: ["Mybatis","MySQL","SQL"]
category: "学习笔记"
updated: 2024-06-07
  
top_group_index: 
---

# MyBatis

MyBatis是一款优秀的**持久层**框架,用于简化JDBC的开发

[官方网站](https://mybatis.org/mybatis-3/zh_CN/index.html)

# 快速入门

步骤:
1. 准备工作(**创建SpringBoot工程,数据库表及其对应的实体类**)

创建SpringBoot工程时,添加依赖:勾选`Mybatis Framework`和`MySQL Driver`

创建对应的数据库表:

```sql
use springboot_web;

create table user
(
    id     int unsigned primary key auto_increment comment 'ID',
    name   varchar(100) comment '姓名',
    age    tinyint unsigned comment '年龄',
    gender tinyint unsigned comment '性别, 1:男, 2:女',
    phone  varchar(11) comment '手机号'
) comment '用户表';

insert into user(id, name, age, gender, phone)
VALUES (null, '白眉鹰王', 55, '1', '18800000000');
insert into user(id, name, age, gender, phone)
VALUES (null, '金毛狮王', 45, '1', '18800000001');
insert into user(id, name, age, gender, phone)
VALUES (null, '青翼蝠王', 38, '1', '18800000002');
insert into user(id, name, age, gender, phone)
VALUES (null, '紫衫龙王', 42, '2', '18800000003');
insert into user(id, name, age, gender, phone)
VALUES (null, '光明左使', 37, '1', '18800000004');
insert into user(id, name, age, gender, phone)
VALUES (null, '光明右使', 48, '1', '18800000005');
```

在com.jinzhao/pojo中创建User类:

```java
package com.jinzhao.pojo;

public class User {
    private Integer id;
    private String name;
    private Short age;
    private Short gender;
    private String phone;

    public User() {
    }

    public User(Integer id, String name, Short age, Short gender, String phone) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.gender = gender;
        this.phone = phone;
    }

    /**
     * 获取
     *
     * @return id
     */
    public Integer getId() {
        return id;
    }

    /**
     * 设置
     *
     * @param id
     */
    public void setId(Integer id) {
        this.id = id;
    }

    /**
     * 获取
     *
     * @return name
     */
    public String getName() {
        return name;
    }

    /**
     * 设置
     *
     * @param name
     */
    public void setName(String name) {
        this.name = name;
    }

    /**
     * 获取
     *
     * @return age
     */
    public Short getAge() {
        return age;
    }

    /**
     * 设置
     *
     * @param age
     */
    public void setAge(Short age) {
        this.age = age;
    }

    /**
     * 获取
     *
     * @return gender
     */
    public Short getGender() {
        return gender;
    }

    /**
     * 设置
     *
     * @param gender
     */
    public void setGender(Short gender) {
        this.gender = gender;
    }

    /**
     * 获取
     *
     * @return phone
     */
    public String getPhone() {
        return phone;
    }

    /**
     * 设置
     *
     * @param phone
     */
    public void setPhone(String phone) {
        this.phone = phone;
    }

    public String toString() {
        return "User{id = " + id + ", name = " + name + ", age = " + age + ", gender = " + gender + ", phone = " + phone + "}";
    }
}
```

2. 引入Mybatis的相关依赖,配置Mybatis(**数据库的连接信息**)

在resources/application.properties中配置:

```properties
spring.application.name=SpringBoot-mybatis-quickstart
# 驱动类名称
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
# 数据库连接的url
spring.datasource.url=jdbc:mysql://192.168.1.13:3306/springboot_web
# 连接数据库的用户名
spring.datasource.username=root
# 连接数据库的密码
spring.datasource.password=123456
```

3. 编写SQL语句(**注解/XML**)

在com.jinzhao/mapper中创建UserMapper接口:

```java
package com.jinzhao.mapper;

import com.jinzhao.pojo.User;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

import java.util.List;

// 在运行时会自动生成该接口的实现类对象(代理对象),并且将该对象交给IOC容器管理
@Mapper
public interface UserMapper {
    // 查询全部用户的信息
    @Select("select id, name, age, gender, phone from user")
    public List<User> list();
}
```

4. 单元测试

```java
package com.jinzhao;

import com.jinzhao.mapper.UserMapper;
import com.jinzhao.pojo.User;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

// SpringBoot整合单元测试的注解
@SpringBootTest
class SpringBootMybatisQuickstartApplicationTests {
    // 通过依赖注入,创建代理对象
    @Autowired
    private UserMapper userMapper;

    @Test
    public void testListUser() {
        List<User> userList = userMapper.list();
        userList.forEach(user -> System.out.println(user.toString()));
    }
}
```

# JDBC

JDBC(Java DataBase Connectivity),是SUN公司提供的一套使用Java语言操作关系型数据库的API(规范)

![JDBC本质](../images/JDBC本质.png)

## JDBC VS MyBatis

1. JDBC程序

```java
@Test
public void testJdbc() throws Exception {
    // 注册驱动
    Class.forName("com.mysql.cj.jdbc.Driver");

    // 获取连接对象
    String url = "jdbc:mysql://192.168.1.13:3306/springboot_web";
    String username = "root";
    String password = "123456";
    Connection connection = DriverManager.getConnection(url, username, password);

    // 获取执行SQL的对象Statement,执行SQL,返回结果
    String sql = "select * from user";
    Statement statement = connection.createStatement();
    ResultSet resultSet = statement.executeQuery(sql);

    // 封装结果数据
    List<User> userList = new ArrayList<>();
    while (resultSet.next()) {
        int id = resultSet.getInt("id");
        String name = resultSet.getString("name");
        short age = resultSet.getShort("age");
        short gender = resultSet.getShort("gender");
        String phone = resultSet.getString("phone");

        User user = new User(id, name, age, gender, phone);
        userList.add(user);
    }

    // 释放资源
    statement.close();
    connection.close();
}
```

2. MyBatis程序

application.properties:

```properties
spring.application.name=SpringBoot-mybatis-quickstart
# 驱动类名称
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
# 数据库连接的url
spring.datasource.url=jdbc:mysql://192.168.1.13:3306/springboot_web
# 连接数据库的用户名
spring.datasource.username=root
# 连接数据库的密码
spring.datasource.password=123456
```

mapper接口:

```java
package com.jinzhao.mapper;

import com.jinzhao.pojo.User;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

import java.util.List;

// 在运行时会自动生成该接口的实现类对象(代理对象),并且将该对象交给IOC容器管理
@Mapper
public interface UserMapper {
    // 查询全部用户的信息
    @Select("select id, name, age, gender, phone from user")
    public List<User> list();
}
```

再通过数据库连接池进行连接复用,减少资源浪费

# 数据库连接池


# lombok