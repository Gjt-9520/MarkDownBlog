---
title: "DML"
date: 2024-05-05
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage7.jpg?raw=true
tags: ["MySQL","SQL"]
category: "学习笔记"
updated: 2024-05-06
swiper_index: 
top_group_index: 
---

# DML

DML全称Data Manipulation Language(数据操作语言),用来对数据库中表的数据记录进行增、删、改操作

1. 添加数据(`INSERT`)
2. 修改数据(`UPDATE`)
3. 删除数据(`DELETE`)

# 添加数据  

## 给指定的字段添加数据

`insert into 表名(字段1,字段2,...) values(值1,值2,...);`

## 给全部字段添加数据

`insert into 表名 values(值1,值2,...);`

## 批量添加数据

`insert into 表名(字段1,字段2,...) values(值1,值2,...),(值1,值2,...),(值1,值2,...);`:为指定字段设置数据

`insert into 表名 values(值1,值2,...),(值1,值2,...),(值1,值2,...);`:为所有字段设置数据

## 细节

1. **插入数据时,指定的字段顺序需要与值的顺序是一一对应的**        
2. **字符串和日期型数据应该包含在引号中**           
3. **插入的数据大小,应该在字段的规定范围内**   

# 修改数据

`update 表名 set 字段名=值1,字段名=值2,...[where 条件];`

## 细节

**修改语句的条件可以有,也可以没有,如果没有条件,则会修改整张表的所有数据**

# 删除数据

`delete from 表名 [where 条件];`

## 细节 

1. **删除语句的条件可以有,也可以没有,如果没有条件,则会删除整张表的所有数据**
2. **删除语句不能删除某一个字段的值(可以使用update,将该字段值置为NULL即可)**
3. **当进行删除全部数据操作时,datagrip会提示,询问是否确认删除,直接点击Execute即可**