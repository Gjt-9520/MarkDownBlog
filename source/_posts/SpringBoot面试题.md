---
title: "SpringBoot面试题"
date: 2024-09-11
description: ""
cover: https://github.com/Gjt-9520/MarkDownBlog/blob/main/source/coverImages/Bimage-135/Bimage127.jpg?raw=true
tags: ["SpringBoot"]
category: "面试"
updated: 2024-09-12
  
top_group_index: 
---

# SpringBoot有什么好处？

1. 简化了原来spring及springmvc一大堆配置文件。

2. 嵌入的Tomcat不需要以war包形式部署。

3. 进行maven依赖包版本统一管理。

# SpringBoot自动配置的原理是什么？

主要是SpringBoot的启动类上的核心注解@SpringBootApplication注解主配置类。

有了这个主配置，类启动时就会为SpringBoot开启一个@EnableAutoConfiguration注解自动配置功能。

有了这个@EnableAutoConfiguration的话就会从配置文件META_INF/Spring.factories加载可能用到的自动配置类。

去重，并将exclude和excludeName属性携带的类排除过滤，将满足条件（@Conditional）的自动配置类返回。

每个类又有自己的一个属性类，每个属性类的属性又从springboot的配置文件中读取。