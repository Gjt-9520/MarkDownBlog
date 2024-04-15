---
title: "Java项目打包"
date: 2024-01-14
description: "在无需安装JDK的情况下也能运行java项目"
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage86.jpg?raw=true
tags: ["打包"]
category: "学习笔记"
updated: 2024-01-15
swiper_index:
top_group_index:
---

# 软件

## exe4j

exe4j是Java可执行文件生成工具  
**exe4j支持的JDK版本是8~11**    

## Inno Setup

Inno Setup是Windows程序免费安装程序   

# 前提

1. 一定要包含图形化界面  
2. 代码及其资源要打包起来  
3. JDK要打包起来  

# 步骤

## 核心步骤

1. 把所有项目代码打包成一个jar包  
2. 把jar包转换成exe的安装包   
3. 把第二步的exe、资源(如图片等)、JDK整合在一起,变成最终的exe安装包  

## 具体步骤

## 代码打包成jar包

创建jar包: 

![项目结构](../images/项目结构.png)

![创建jar1](../images/创建jar1.png)

![创建jar2](../images/创建jar2.png)

![创建jar3](../images/创建jar3.png)

构建工件(Artifact): 

![构建工件1](../images/构建工件1.png)

![构建工件2](../images/构建工件2.png)

jar包默认位置: out/artifacts/XXX_jar/XXX.jar: 

![jar包默认位置](../images/jar包默认位置.png)

## 整合资源文件

1. 将创建好的jar包拷贝到桌面上
2. 在桌面上新建一个文件夹resource
3. 将项目中的资源复制到文件夹resource

## 将jar包打包成exe安装包

打开exe4j软件: 

![jar转exe模式](../images/jar转exe模式.png)

![名称和路径](../images/名称和路径.png)

![生成64位1](../images/生成64位1.png)

![生成64位2](../images/生成64位2.png)

![生成64位3](../images/生成64位3.png)

![添加jar1](../images/添加jar1.png)

![添加jar2](../images/添加jar2.png)

![添加jar3](../images/添加jar3.png)

![添加jar4](../images/添加jar4.png)

![添加jar5](../images/添加jar5.png)

![添加jar6](../images/添加jar6.png)

![添加jar7](../images/添加jar7.png)

![添加jar8](../images/添加jar8.png)

![添加jar9](../images/添加jar9.png)

![添加jar10](../images/添加jar10.png)

![添加jar11](../images/添加jar11.png)

![添加jar12](../images/添加jar12.png)

等待加载: 

![添加jar13](../images/添加jar13.png)

![添加jar14](../images/添加jar14.png)

点击Exit后,会提升是否需要保存刚刚的配置信息,可以点击Yes,并选择一个路径进行保存: 

![添加jar15](../images/添加jar15.png)

## 将jdk、资源文件、jar包转换后的exe三者再次打包成最终的exe

打开Inno Setup软件: 

![创建安装程序1](../images/创建安装程序1.png)

![创建安装程序2](../images/创建安装程序2.png)

![创建安装程序3](../images/创建安装程序3.png)

![创建安装程序4](../images/创建安装程序4.png)

![创建安装程序5](../images/创建安装程序5.png)

![创建安装程序6](../images/创建安装程序6.png)

![创建安装程序7](../images/创建安装程序7.png)

![创建安装程序8](../images/创建安装程序8.png)

![创建安装程序9](../images/创建安装程序9.png)

![创建安装程序10](../images/创建安装程序10.png)

![创建安装程序11](../images/创建安装程序11.png)

![创建安装程序12](../images/创建安装程序12.png)

![创建安装程序13](../images/创建安装程序13.png)

![创建安装程序14](../images/创建安装程序14.png)

![创建安装程序15](../images/创建安装程序15.png)

![创建安装程序16](../images/创建安装程序16.png)

添加脚本1: `#define MyJdkName "jdk"`     
添加脚本2: `Source:  "自己本地JDK路径\*"; DestDir:  "{app}\{#MyJdkName}"; Flags:  ignoreversion recursesubdirs createallsubdirs`    

![创建安装程序17](../images/创建安装程序17.png)

![创建安装程序18](../images/创建安装程序18.png)

点击是,选择一个位置保存:   

![创建安装程序19](../images/创建安装程序19.png)

当绿色滚动条结束后,会自动安装exe文件    
此时可以点击否,先不安装      
在桌面上,会多了一个XXX.exe文件和一个后缀名为iss的文件    
  
XXX.exe: 打包成功的项目安装包    
iss文件: 刚刚设置的脚本文件    

# 安装

点击XXX.exe安装项目,并在安装目录中找到XXX.exe即可在无需安装JDK的情况下运行项目   