---
title: "Mybatis面试题"
date: 2024-09-13
description: ""
cover: https://github.com/Gjt-9520/MarkDownBlog/blob/main/source/coverImages/Bimage-135/Bimage129.jpg?raw=true
tags: ["Mybatis"]
category: "面试"
updated: 2024-09-14
  
top_group_index: 
---

# MyBatis实现一对一/多对一有几种方式？具体怎么操作的？

1. 级联映射（association）

在同一个SQL查询中一次性获取所有需要的数据，并通过association标签进行映射

2. 分步查询（延迟加载）

先查询主表，然后在需要时再查询关联表，这种方式可以提高性能，因为只有在实际访问关联对象时才会执行额外的查询

# MyBatis实现一对多有几种方式？具体怎么操作的？

1. 联合查询（collection）

在同一个SQL查询中一次性获取所有需要的数据，并通过collection标签进行映射

2. 分步查询（延迟加载）

先查询主表，然后在需要时再查询关联表，这种方式可以提高性能，因为只有在实际访问关联对象时才会执行额外的查询

# `#{}`和`${}`的区别?

- #{}是预编译,标记一个占位符,可以防止sql注入

底层使用PreparedStatement

特点：

先进行SQL语句的编译，然后给SQL语句的占位符问号？传值

给？传值时是否添加 '' 根据传递的数据类型

可以避免SQL注入的风险

- ${}是动态拼接,在动态sql中拼接字符串,可能导致sql注入

底层使用Statement

特点：先进行SQL语句的拼接，然后再对SQL语句进行编译

存在SQL注入的风险

# Mybatis的一级、二级缓存?

1. 一级缓存: 基于PerpetualCache的HashMap本地缓存，其存储作用域为Session，当Session flush或close之后，该Session中的所有Cache就将清空，默认打开一级缓存

2. 二级缓存与一级缓存其机制相同，默认也是采用PerpetualCache的HashMap存储，不同在于其存储作用域为Mapper(Namespace)，并且可自定义存储源，默认不开启二级缓存缓存

# Mybatis Mapper接口支持重载吗？

不支持

因为mybatis是根据package+Mapper+method全限名作为key，去xml内寻找唯一sql来执行的

如果方法名一样会导致找到两个sql，报错

# Mybatis什么时候用的${}？

动态表名字以及ordery by后面的字段，因为#{}传过来的参数带单引号''，而${}传过来的参数不带单引号。

# 当实体类中的属性名和表中的字段名不一样，怎么办？

1. 通过在查询的sql语句中定义字段名的别名，让字段名的别名和实体类的属性名一致

2. 通过resultmap来映射字段名和实体类属性名的对应的关系

# Mybatis是如何进行分页的？分页插件的原理是什么？

Mybatis使用RowBounds对象进行逻辑分页，它是针对ResultSet结果集执行的内存分页

分页插件的基本原理是使用Mybatis提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的sql

然后重写sql，添加对应的物理分页语句和物理分页参数