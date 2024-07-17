---
title: "Docker安装MySQL"
date: 2024-07-22
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage77.jpg?raw=true
tags: ["Docker","MySQL"]
category: "配置"
updated: 2024-07-23
  
top_group_index: 
---

# 下载镜像

- [Docker Hub官方网站](https://hub.docker.com/)

- `docker pull mysql`:下载最新版Mysql镜像(其实此命令就等同于`docker pull mysql:latest`)

- `docker pull mysql:xxx`:下载指定版本的Mysql镜像(xxx指具体版本号)

# 检查Docker下载的所有镜像

`docker images`

# 创建网络

没有指定网络,可以先创建网络:`docker network create net`

# 创建mysql目录

1. 在root目录下创建mysql目录

`cd /root`

`mkdir mysql`

2. 在mysql目录下创建文件夹log、data、conf、init

**log、data、conf、init分别对应mysql的日志文件、储存文件、配置文件、初始化文件**

`cd mysql`

`mkdir log data conf init`

# 创建容器并运行

```cmd
docker run \
  -d \
  --name mysql \
  -p 3306:3306 \
  -e TZ=Asia/Shanghai \
  -e MYSQL_ROOT_PASSWORD=123456 \
  -v /root/mysql/log:/var/log/mysql \
  -v /root/mysql/data:/var/lib/mysql \
  -v /root/mysql/conf:/etc/mysql/conf.d \
  -v /root/mysql/init:/docker-entrypoint-initdb.d \
  --network net \
  mysql
```

解释:
- `docker run`:运行容器
- `-d`:后台运行容器
- `--name mysql`:指定容器名称为mysql
- `-p 3306:3306`:将容器的3306端口映射到主机的3306端口
- `-e TZ=Asia/Shanghai`:设置时区为亚洲上海
- `-e MYSQL_ROOT_PASSWORD=123456`:设置root用户的密码为123456
- `-v /root/mysql/log:/var/log/mysql`:将主机的/root/mysql/log目录挂载到容器的/var/log/mysql目录
- `-v /root/mysql/data:/var/lib/mysql`:将主机的/root/mysql/data目录挂载到容器的/var/lib/mysql目录
- `-v /root/mysql/conf:/etc/mysql`:将主机的/root/mysql/conf目录挂载到容器的/etc/mysql目录
- `-v /root/mysql/init:/docker-entrypoint-initdb.d`:将主机的/root/mysql/init目录挂载到容器的/docker-entrypoint-initdb.d目录
- `--network mysql-net`:指定容器的网络为net
- `--restart unless-stopped`:容器退出后自动重启
- `mysql`:指定镜像名称为mysql

# 检查mysql运行状态

- `docker ps`:检查容器是否运行
- `docker inspect mysql`:查看容器详细信息

# 使用mysql

- `docker exec -it mysql bash`:进入mysql容器
- `mysql -uroot -p123456`:登录mysql