---
title: "MySQL日志"
date: 2024-05-23
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage25.jpg?raw=true
tags: ["MySQL"]
category: "学习笔记"
updated: 2024-05-24
swiper_index: 
top_group_index: 
---

# 错误日志

错误日志是MySQL中最重要的日志之一,它记录了当mysqld启动和停止时,以及服务器在运行过程中发生任何严重错误时的相关信息               
当数据库出现任何故障导致无法正常使用时,建议首先查看此日志                  
该日志是默认开启的,默认存放目录`/var/log/`,默认的日志文件名为mysqld.log        
查看日志位置:`show variables like '%log_error%';`




# 二进制日志

# 查询日志

# 慢查询日志