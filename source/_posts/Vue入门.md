---
title: "Vue入门"
date: 2024-05-31
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage33.jpg?raw=true
tags: ["Vue","Element","Nginx"]
category: "笔记"
updated: 2024-06-01
 
top_group_index: 
---

# 前后端分离开发

![前后端分离开发](../images/前后端分离开发.png)

# 前端工程化

![前端工程化](../images/前端工程化.png)

# Vue项目

## 创建项目

到目标文件夹的终端中输入`vue ui`,设置并创建项目

## 目录结构

![Vue项目目录结构](../images/Vue项目目录结构.png)

## 启动项目

1. VSCode

![NPM脚本](../images/Vue项目启动.png)

2. 终端输入:`npm run serve`

## 设置端口

在vue.config.js文件中修改设置端口号

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  devServer: {
    port: 7000,
  }
})
```

## 开发流程

![Vue发开流程1](../images/Vue发开流程1.png)

![Vue发开流程2](../images/Vue发开流程2.png)

## 组件库

Element:饿了么团队研发的,一套为开发者、设计师和产品经理准备的基于Vue2.0的桌面端**组件库**

组件:组成网页的部件,例如超链接、按钮、图片、表格、表单、分页条等

[Element官方网站](https://element-plus.org/zh-CN/)

### 快速入门

![快速入门](../images/Element快速入门.png)

## 打包部署

### 打包

![打包](../images/Vue打包.png)

### 部署

![Nginx1](../images/Nginx1.png)

[Nginx官方网站](https://nginx.org/)

![Nginx2](../images/Nginx2.png)

![部署](../images/Nginx部署.png)