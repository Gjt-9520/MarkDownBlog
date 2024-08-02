---
title: "安装RabbitMQ"
date: 2024-07-25
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage83.jpg?raw=true
tags: ["Docker","RabbitMQ"]
category: "配置"
updated: 2024-07-26
  
top_group_index: 
---

# 下载镜像

- [Docker Hub官方网站](https://hub.docker.com/)

- `docker pull rabbitmq`:下载最新版RabbitMQ镜像(其实此命令就等同于`docker pull rabbitmq:latest`)

- `docker pull rabbitmq:xxx`:下载指定版本的RabbitMQ镜像(xxx指具体版本号)

# 检查镜像

`docker images`

# 创建网络

如果没有指定网络,可以先创建网络:`docker network create net`

# 创建容器并运行

```cmd
docker run \
 -d \
 --name rabbitmq \
 --hostname rabbitmq \
 -e RABBITMQ_DEFAULT_USER=rabbitmq \
 -e RABBITMQ_DEFAULT_PASS=rabbitmq \
 -v mq-plugins:/plugins \
 -p 15672:15672 \
 -p 5672:5672 \
 --network net \
 rabbitmq:3.8-management
```

解释:
- `docker run`:运行容器
- `-d`:后台运行容器
- `--name rabbitmq`:指定容器名称为rabbitmq
- `--hostname rabbitmq`:指定容器主机名为rabbitmq
- `-e RABBITMQ_DEFAULT_USER=rabbitmq`:设置默认用户名为rabbitmq
- `-e RABBITMQ_DEFAULT_PASS=rabbitmq`:设置默认密码为rabbitmq
- `-v mq-plugins:/plugins`:将本地的mq-plugins文件夹(数据卷)挂载到容器的/plugins路径下
- `-p 15672:15672`:将主机的15672端口映射到容器的15672端口
- `-p 5672:5672`:将主机的5672端口映射到容器的5672端口
- `--network net`:指定容器的网络为net
- `rabbitmq:3-management`:指定镜像名称为rabbitmq:3.8-management

# 检查rabbitmq运行状态

- `docker ps`:检查容器是否运行
- `docker inspect rabbitmq`:查看容器详细信息

确认rabbitmq运行之后,即可在浏览器中输入`http://192.168.149.100:15672`(其中`192.168.149.100`为rabbitmq所在主机的ip地址,需替换成自己的)

访问rabbitmq的控制台,用户名和密码都是rabbitmq

# 安装DelayExchange插件

1. 查看RabbitMQ的插件目录对应的数据卷

`docker volume ls`

2. 进入RabbitMQ的插件目录

`cd /var/lib/docker/volumes/mq-plugins/_data`

3. 下载DelayExchange插件到挂载的插件目录下

[官网插件地址](https://www.rabbitmq.com/community-plugins#overview)

[Github插件地址](https://github.com/rabbitmq/rabbitmq-delayed-message-exchange/releases)

4. 安装插件

`docker exec -it rabbitmq rabbitmq-plugins enable rabbitmq_delayed_message_exchange`