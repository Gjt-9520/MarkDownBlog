---
title: "DML"
date: 2024-05-05
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage7.jpg?raw=true
tags: ["MySQL","SQL"]
category: "学习笔记"
updated: 2024-05-06
 
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

1. `insert into 表名(字段1,字段2,...) values(值1,值2,...),(值1,值2,...),(值1,值2,...);`:为指定字段设置数据
2. `insert into 表名 values(值1,值2,...),(值1,值2,...),(值1,值2,...);`:为所有字段设置数据

## 细节

1. **插入数据时,指定的字段顺序需要与值的顺序是一一对应的**        
2. **字符串和日期型数据应该包含在引号中**           
3. **插入的数据大小,应该在字段的规定范围内**     

# 修改数据

`update 表名 set 字段名=值1,字段名=值2,...[where 条件];`

## 细节

1. **修改语句的条件可以有,也可以没有,如果没有条件,则会修改整张表的所有数据**
2. **当进行修改全部数据操作时,会提示询问是否确认修改,直接点击Execute即可**

# 删除数据

`delete from 表名 [where 条件];`

## 细节 

1. **删除语句的条件可以有,也可以没有,如果没有条件,则会删除整张表的所有数据**
2. **删除语句不能删除某一个字段的值(可以使用update,将该字段值置为NULL即可)**
3. **当进行删除全部数据操作时,会提示询问是否确认删除,直接点击Execute即可**

# 范例

```sql
-- 创建员工表
create table tb_emp
(
    id          int auto_increment comment '主键ID'
        primary key,
    username    varchar(20)                  not null comment '用户名',
    password    varchar(32) default '123456' not null comment '密码',
    name        varchar(10)                  not null comment '姓名',
    gender      tinyint unsigned             not null comment '性别(1 男,2 女)',
    image       varchar(300)                 null comment '图像url',
    job         tinyint unsigned             null comment '职位(1 讲师,2 班主任,3 就业指导)',
    entry_date  date                         null comment '入职日期',
    create_time datetime                     not null comment '创建时间',
    update_time datetime                     not null comment '修改时间',
    constraint tb_emp_pk2
        unique (username)
)
    comment '员工表';

-- 为指定字段插入数据
insert into tb_emp(username, name, gender, create_time, update_time)
values ('zhangsan', '张三', 1, now(), now());

-- 为所有字段插入数据
insert into tb_emp(id, username, password, name, gender, image, job, entry_date, create_time, update_time)
values (null, 'lisi', '111111', '李四', 1, '1.jpg', 2, '2022-01-01', now(), now());

insert into tb_emp
values (null, 'wangwu', '123', '王五', 1, '2.jpg', 1, '2022-02-01', now(), now());

-- 批量插入数据
insert into tb_emp(username, name, gender, create_time, update_time)
values ('zhangsan1', '张三1', 1, now(), now()),
       ('zhangsan2', '张三2', 1, now(), now()),
       ('zhangsan3', '张三3', 1, now(), now());

-- 为指定字段修改数据
update tb_emp
set username='zhangsan111',
    name='张三111',
    gender=2
where username = 'zhangsan1';

-- 为所有字段修改数据
update tb_emp
set entry_date='2022-02-02',
    update_time=now();

-- 为指定字段删除数据
delete
from tb_emp
where id = 1;

-- 为所有字段删除数据
delete
from tb_emp;
```