---
title: "Docker Compose一键部署"
date: 2024-07-26
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage82.jpg?raw=true
tags: ["Docker","Mysql","Nginx","Nacos","Sentinel","Seata","RabbitMQ"]
category: "配置"
updated: 2024-07-27
  
top_group_index: 
---

# 准备工作

将MySQL、Nginx、Nacos、Sentinel、Seata、RabbitMQ在容器运行前的准备工作做好(参考Docker安装系列文章)

# Docker Compose一键部署

将docker-compose.yml文件放到root目录下

```yml
version: '3.8'

services:
  mysql:
    image: mysql
    container_name: mysql
    ports:
      - "3306:3306"
    environment:
      - TZ=Asia/Shanghai
      - MYSQL_ROOT_PASSWORD=123456
    volumes:
      - /root/mysql/log:/var/log/mysql
      - /root/mysql/data:/var/lib/mysql
      - /root/mysql/conf:/etc/mysql/conf.d
      - /root/mysql/init:/docker-entrypoint-initdb.d
    networks:
      - net

  nginx:
    image: nginx
    container_name: nginx
    ports:
      - "80:80"
    volumes:
      - /root/nginx/conf/nginx.conf:/etc/nginx/nginx.conf
      - /root/nginx/conf/conf.d:/etc/nginx/conf.d
      - /root/nginx/log:/var/log/nginx
      - /root/nginx/html:/usr/share/nginx/html
    depends_on:
      - mysql
    networks:
      - net

  nacos:
    image: nacos/nacos-server
    container_name: nacos
    env_file:
      - /root/nacos/custom.env
    ports:
      - "8848:8848"
      - "9848:9848"
      - "9849:9849"
    depends_on:
      - nginx
    networks:
      - net

  sentinel:
    image: bladex/sentinel-dashboard
    container_name: sentinel
    ports:
      - "8858:8858"
    depends_on:
      - nacos
    networks:
      - net

  seata:
    image: seataio/seata-server:1.5.2
    container_name: seata
    ports:
      - "8091:8091"
      - "7091:7091"
    environment:
      - SEATA_IP=192.168.149.100
    volumes:
      - /root/seata:/seata-server/resources
    privileged: true
    depends_on:
      - sentinel
    networks:
      - net

  rabbitmq:
    image: rabbitmq:3.8-management
    container_name: rabbitmq
    hostname: rabbitmq
    ports:
      - "15672:15672"
      - "5672:5672"
    environment:
      - RABBITMQ_DEFAULT_USER=rabbitmq
      - RABBITMQ_DEFAULT_PASS=rabbitmq
    volumes:
      - mq-plugins:/plugins
    depends_on:
      - seata
    networks:
      - net

volumes:
  mq-plugins:

networks:
  net:
```

- `docker compose up -d`:创建并启动所有服务容器
- `docker compose down`:停止并移除所有服务容器和网络

- `docker compose ps`:查看所有服务容器状态
- `docker compose logs -f`:查看所有服务容器日志

- `docker compose start`:启动所有服务容器
- `docker compose stop`:停止所有服务容器
- `docker compose restart`:重启所有服务容器