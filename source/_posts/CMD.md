---
title: "CMD"
date: 2023-11-25
description: "Windows命令提示符"
cover: https://github.com/Gjt-9520/ImageResources/blob/main/photos/original/Ximage77.jpg?raw=true
tags: ["Terminal"]
category: "学习笔记"
updated: 2023-11-25
---

## CMD技巧

### 查看帮助

`help` -- 查看帮助   

### 关机、重启、注销、休眠、定时

`shutdown -s` -- 关机   
`shutdown -r` -- 重启   
`shutdown -l` -- 关机   
`shutdown -h -f` -- 休眠   
`shutdown -a` -- 取消关机   
`shutdown -s -t 60` -- 60秒后关机   

### 目录操作

#### 切换目录,进入指定文件夹

`d:` -- 切换到磁盘d   
`d:/image` -- 切换到磁盘d的image文件夹    
`cd image` -- 切换到image文件夹   
`cd /` -- 返回根目录    
`cd ..` -- 返回上级目录   
`md image` -- 新建image文件夹   

#### 显示目录内容

`dir` -- 显示目录中的文件列表   
`tree d:/image` -- 显示d盘image文件夹的目录结构   
`cd` -- 显示当前目录位置   
`cd d:` -- 显示指定磁盘的当前目录位置    

### 网络操作

`ping ip/域名` -- 延迟和丢包率   
`ping ip/域名 -n 5` -- `Ping`测试5次   
`ipconfig /flushdns` -- 清除本地`DNS`缓存   
`tracert ip/域名` -- 路由追踪   

### 进程/服务操作

#### 进程管理

`tasklist` -- 显示当前正在运行的进程   
`start notepad` -- 运行程序/命令(例如记事本)   
`taskkill /im notepad.exe` -- 按照名称,结束进程(例如记事本)    
`taskkill /pid 1234` -- 按照`PID`,结束进程(例如1234)   
  
#### 服务管理

`net start` -- 显示当前正在运行的服务    
`net start 服务名` -- 启动指定服务   
`net stop 服务名` -- 停止指定服务    
  
### 保存为`.bat`可执行文件

可以将常用的命令输入记事本中,并保存为后缀为`.bat`的可执行文件    
双击该文件即可执行指定命令; 将文件放入系统启动目录中,可以实现开机自动运行       

启动目录位置: `C:\Users\用户名\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`    