---
title: "HTTP协议"
date: 2024-06-03
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage36.jpg?raw=true
tags: ["Web","HTTP"]
category: "学习笔记"
updated: 2024-06-04
 
top_group_index: 
---

# HTTP

概念:Hyper Text Transfer Protocol,超文本传输协议,规定了浏览器和服务器之间数据传输的规则

![HTTP协议概念](../images/HTTP概念.png)

特点:
1. 基于TCP协议:面向连接,安全
2. 基于请求-响应模型:一次请求对应一次响应
3. HTTP协议是**无状态**的协议:对于事务处理没有记忆能力,每次请求-响应都是独立的                 
优点:速度快                   
缺点:多次请求键间不能共享数据                

细节:**HTTP协议的默认端口号为80**

# 请求协议

![HTTP请求数据格式](../images/HTTP请求数据格式.png)

请求头:

![HTTP请求头](../images/HTTP请求头.png)
  
# 响应协议

![HTTP响应数据格式](../images/HTTP响应数据格式.png)

状态码:

![HTTP响应状态码](../images/HTTP响应状态码.png)

常见的响应状态码:

![常见的响应状态码](../images/常见的响应状态码.png)

[状态码大全](https://cloud.tencent.com/developer/chapter/13553)

响应头:

![HTTP响应头](../images/HTTP响应头.png)

# 协议解析

通过Web服务器进行HTTP协议解析,例如Tomcat