---
title: "Docker安装Redis及搭建Redis主从集群"
date: 2024-08-03
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage92.jpg?raw=true
tags: ["Redis","Docker"]
category: "配置"
updated: 2024-08-04
  
top_group_index: 
---

# 下载镜像

- [Docker Hub官方网站](https://hub.docker.com/)

- `docker pull redis`:下载最新版Redis镜像

# 检查镜像

`docker images`

# 安装Redis




# 搭建Redis主从集群


## Docker Compose创建Redis容器

```yaml
version: "3.2"

services:
  r1:
    image: redis
    container_name: r1
    network_mode: "host"
    entrypoint: ["redis-server", "--port", "7001"]
    volumes:
      - ./7001/conf:/etc/redis/redis.conf
      - ./7001/data:/data
  r2:
    image: redis
    container_name: r2
    network_mode: "host"
    entrypoint: ["redis-server", "--port", "7002"]
    volumes:
      - ./7002/conf:/etc/redis/redis.conf
      - ./7002/data:/data
  r3:
    image: redis
    container_name: r3
    network_mode: "host"
    entrypoint: ["redis-server", "--port", "7003"]
    volumes:
      - ./7003/conf:/etc/redis/redis.conf
      - ./7003/data:/data
```

解释:
- `network_mode: "host"`:设置容器使用主机的网络模式
- `entrypoint: ["redis-server", "--port", "7002"]`:设置容器启动时执行的命令为redis-server,并指定不同的端口号

## 建立集群

命令:
- Redis5.0以前:`slaveof <masterip> <masterport>`
- Redis5.0以后:`replicaof <masterip> <masterport>`

两种模式:
- 永久生效:在redis.conf文件中利用slaveof命令指定master节点
- 临时生效:直接利用redis-cli控制台输入slaveof命令,指定master节点

这里使用临时模式,步骤如下:

1. 连接r2节点:`docker exec -it r2 redis-cli -p 7002`
2. 认r1主,也就是7001:`slaveof 192.168.1.8 7001`
3. 查看r2节点是否成功认r1主:`info replication`
4. 退出r2节点:`exit`

5. 连接r3节点:`docker exec -it r3 redis-cli -p 7003`
6. 认r1主,也就是7001:`slaveof 192.168.1.8 7001`
7. 查看r3节点是否成功认r1主:`info replication`
8. 退出r3节点:`exit`

9. 验证:`docker exec -it r1 redis-cli -p 7001`