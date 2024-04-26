---
title: "Gson"
date: 2023-12-24
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage84.jpg?raw=true
tags: ["Java SE","工具包"]
category: "学习笔记"
updated: 2023-12-25
swiper_index:
top_group_index: 
---

# Gson

## 深克隆   

```java
Gson gson = new Gson();
        // 把对象变成一个字符串
        String str = gson.toJson(user1);
        // 再把字符串变回对象
        User user2 = gson.fromJson(str,User.class);
```

范例:     

```java
import com.google.gson.Gson;

public class CloneUser {
    public static void main(String[] args) throws CloneNotSupportedException {
        // 定义user1的游戏进度data1
        int[] data1 = {1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,0};
        // 创建用户对象
        User user1 = new User(1,"张三","123456","animal7",data1);
        // 打印结果:"原对象数组:角色编号为：1,用户名为：张三密码为：123456,游戏图片为:animal7,游戏进度:[1 ,2 ,3 ,4 ,5 ,6 ,7 ,8 ,9 ,10 ,11 ,12 ,13 ,14 ,15 ,0 ]"
        System.out.println("原对象数组:" + user1);

        Gson gson = new Gson();
        // 把对象变成一个字符串
        String str = gson.toJson(user1);
        // 再把字符串变回对象
        User user2 = gson.fromJson(str,User.class);
        
        data1[0] = 100;
        // 打印结果:"原对象数组:角色编号为：1,用户名为：张三密码为：123456,游戏图片为:animal7,游戏进度:[100 ,2 ,3 ,4 ,5 ,6 ,7 ,8 ,9 ,10 ,11 ,12 ,13 ,14 ,15 ,0 ]"
        System.out.println("原对象数组:" + user1);
        // 打印结果:"克隆对象数组:角色编号为：1,用户名为：张三密码为：123456,游戏图片为:animal7,游戏进度:[1 ,2 ,3 ,4 ,5 ,6 ,7 ,8 ,9 ,10 ,11 ,12 ,13 ,14 ,15 ,0 ]"
        System.out.println("克隆对象数组:" + user2);
    }
}
```