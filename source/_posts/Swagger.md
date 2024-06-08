---
title: "Swagger"
date: 2024-06-22
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage49.jpg?raw=true
tags: ["SpringBoot","API文档"]
category: "学习笔记"
updated: 2024-06-23
  
top_group_index: 
---

# Swagger

使用Swagger,只需要按照它的规范去定义接口及接口相关的信息,就可以做到生成接口文档,以及**在线接口调试**页面

[Swagger官方网站](https://swagger.io/)

Knife4j是为Java MVC框架集成Swagger生成Api文档的增强解决方案

# Knife4j使用步骤

1. 导入Knife4j的maven坐标

```xml
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>3.0.2</version>
</dependency>
```

2. 在配置类(WebMvcConfiguration.java)中加入Knife4j相关配置,并设置静态资源映射

```java
/**
 * 通过knife4j生成接口文档
 * @return
 */
@Bean
public Docket docket() {
    log.info("准备生成接口文档...");
    ApiInfo apiInfo = new ApiInfoBuilder()
            .title("苍穹外卖项目接口文档")
            .version("2.0")
            .description("苍穹外卖项目接口文档")
            .build();
    Docket docket = new Docket(DocumentationType.SWAGGER_2)
            .apiInfo(apiInfo)
            .select()
            //指定生成接口需要扫描的包
            .apis(RequestHandlerSelectors.basePackage("com.sky.controller"))
            .paths(PathSelectors.any())
            .build();
    return docket;
}

/**
 * 设置静态资源映射
 * @param registry
 */
protected void addResourceHandlers(ResourceHandlerRegistry registry) {
    log.info("开始设置静态资源映射...");
    registry.addResourceHandler("/doc.html").addResourceLocations("classpath:/META-INF/resources/");
    registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-INF/resources/webjars/");
}
```

3. 通过`localhost:8080/doc.html`进入接口文档

# ApiFox和Swagger

- ApiFox是设计阶段使用的工具,管理和维护接口
- Swagger在开发阶段使用的框架,帮助后端开发人员做后端的接口测试

# 常用注解

![Swagger常用注解](../images/Swagger常用注解.png)


