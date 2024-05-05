---
title: "XML"
date: 2024-04-28
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage1.jpg?raw=true
tags: ["Java SE"]
category: "学习笔记"
updated: 2024-04-29
swiper_index: 
top_group_index: 
---

# 配置文件

配置文件用来保存程序在运行时所需要的一些参数

常见的有:

1. `*.txt`             
优点:没有优点                
缺点:不利于阅读          

2. `*.properties`                    
优点:键值对形式利于阅读,解析简单                  
缺点:无法配置一组一组的数据            

3. `*.xml`                 
优点:易于阅读,可以配置成组出现的数据             
缺点:解析比较复杂               

细节:**如果数据量较少,一个键只对应一个值,使用properties;如果数据量较多,使用xml**

# XML

XML的全称(Extensible Markup Language),是一种**可扩展的标记语言**

标记语言:通过标签来描述数据的一门语言(标签有时也称之为元素)              

可扩展:标签的名字是可以自己定义的

作用:
1. 用于进行存储数据和传输数据
2. 作为软件的配置文件

# 创建



