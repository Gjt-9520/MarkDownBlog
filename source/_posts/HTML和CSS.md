---
title: "HTML和CSS"
date: 2024-05-27
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage29.jpg?raw=true
tags: ["Web","前端"]
category: "学习笔记"
updated: 2024-05-28
swiper_index: 
top_group_index: 
---

# Web

## Web网站开发模式

![Web网站开发模式](../images/web网站开发模式.png)

前端的代码是如何转换成用户眼中的网页的?

通过浏览器转化(解析和渲染)成用户看到的网页

浏览器中对代码进行解析和渲染的部分,称为**浏览器内核**

[前端学习网站w3school](https://www.w3school.com.cn/)

## Web标准

**Web标准**也称为**网页标准**,由一系列的标准组成,大部分由W3C(World Wide Web Consortium,万维网联盟)负责制定

三个组成部分:
- HTML:负责**网页的结构**(页面元素和内容)
- CSS:负责**网页的表现**(页面元素的外观、位置等页面样式,如:颜色、大小等)
- JavaScript:负责**网页的行为**(交互效果)

# HTML

HTML(HyperText Markup Language):超文本标记语言

- 超文本:超越了文本的限制,比普通文本更强大,除了文字信息,还可以定义图片、音频、视频等内容             
- 标记语言:由标签构成的语言
- HTML标签都是**预定义**好的,例如:使用`<h1>`标签展示标题,使用`<a>`展示超链接,使用`<img>`展示图片,`<video>`展示视频
- HTML代码直接在浏览器中运行,HTML标签由浏览器解析

## 快速入门范例

```html
<html>
    <head>
        <title>网站</title>
    </head>
    <body>
        <h1>hello,world</h1>
        <img src="image\234781255.jpg"/>
    </body>
</html>
```

HTML中的标签特点:
- HTML标签不区分大小写
- HTML标签的属性值,采用单引号、双引号都可以
- HTML语法相对比较松散(建议大家编写HTML标签的时候尽量严谨一些)

# CSS

CSS(Cascading Style Sheet),层叠样式表,用于控制页面的样式(表现)

## CSS引入方式

- 行内样式:写在标签的style属性中(不推荐)
- 内嵌样式:写在style标签中(可以写在页面任何位置,但通常约定写在head标签中)
- 外联样式:写在一个单独的.css文件中(需要通过link标签在网页中引入)

# 基础标签和样式

## 图片标签

`<img>`

- `src`:指定图像的url(绝对路径/相对路径)
- `alt`:指定图像的名称
- `width`:图像的宽度(像素px/相对于父元素的百分比%)
- `height`:图像的高度(像素px/相对于父元素的百分比%)

## 标题标签

`<h1>`、`<h2>`、`<h3>`、`<h4>`、`<h5>`、`<h6>`

## 水平分割线

`<hr>`

