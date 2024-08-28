---
title: "Http常见状态码"
date: 2024-03-25
description: ""
cover: https://github.com/Gjt-9520/MarkDownBlog/blob/main/source/coverImages/Bimage-135/Bimage114.jpg?raw=true
tags: ["Http"]
category: "笔记"
updated: 2024-03-26
  
top_group_index: 
---

# 状态码

状态码的职责是当客户端向服务器端发送请求时,描述返回的请求结果

借助状态码,用户可以知道服务器端是正常处理了请求,还是出现了错误

# 1XX 信息性状态码

**接收的请求正在处理**

# 2XX 成功状态码

**请求正常,处理完毕**

- `200` ok:请求成功

- `204` no content:请求成功,但是没有结果返回

- `206` partial content:客户端请求一部分资源,服务端成功响应,返回一范围资源

# 3XX 重定向状态码

**需要进行附加操作以完成请求**

- `301` move permanently:永久性重定向

- `302` found:临时性重定向

- `303` see other:示由于请求对应的资源存在着另一个url,应使用GET方法定向获取请求的资源

- `304` not modified:表示在客户端采用带条件的访问某资源时,服务端找到了资源,但是这个请求的条件不符合,跟重定向无关

- `307` temporary redirect:临时性重定向

# 4XX 客户端错误

**服务器无法处理请求**

- `400` bad request:请求报文存在语法错误

- `401` unauthorized:需要认证(第一次返回)或者认证失败(第二次返回)

- `403` forbidden:请求被服务器拒绝了

- `404` not found:服务器上无法找到请求的资源

# 5XX 服务器错误

- `500` internal server error:服务端执行请求时发生了错误

- `503` service unavailable:服务器正在超负载或者停机维护,无法处理请求