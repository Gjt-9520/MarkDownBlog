---
title: "Docker安装Sentinel"
date: 2024-07-24
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage79.jpg?raw=true
tags: ["Docker","Sentinel"]
category: "配置"
updated: 2024-07-25
  
top_group_index: 
---

# 下载镜像

- [Docker Hub官方网站](https://hub.docker.com/)

- `docker pull bladex/sentinel-dashboard`:下载最新版Sentinel镜像

# 检查镜像

`docker images`

# 创建网络

如果没有指定网络,可以先创建网络:`docker network create net`

# 创建容器并运行

```cmd
docker run \
 -d \
 --name sentinel \
 -p 8858:8858 \
 --network net \
 bladex/sentinel-dashboard
```

解释:
- `docker run`:运行容器
- `-d`:后台运行容器
- `--name sentinel`:指定容器名称为sentinel
- `-p 8858:8858`:将容器的8858端口映射到主机的8858端口
- `--network net`:指定容器的网络为net
- `bladex/sentinel-dashboard`:指定镜像名称为bladex/sentinel-dashboard

# 检查sentinel运行状态

- `docker ps`:检查容器是否运行
- `docker inspect sentinel`:查看容器详细信息

确认sentinel运行之后,即可在浏览器中输入`http://192.168.149.100:8858`(其中`192.168.149.100`为sentinel所在主机的ip地址,需替换成自己的)

访问sentinel的控制台,用户名和密码都是sentinel