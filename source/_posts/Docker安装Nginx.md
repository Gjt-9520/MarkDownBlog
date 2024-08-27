---
title: "安装Nginx"
date: 2024-07-22
description: ""
cover: https://github.com/Gjt-9520/MarkDownBlog/blob/main/source/coverImages/Bimage-135/Bimage81.jpg?raw=true
tags: ["Docker","Nginx"]
category: "配置"
updated: 2024-07-23
  
top_group_index: 
---

# 下载镜像

- [Docker Hub官网](https://hub.docker.com/)

- `docker pull nginx`:下载最新版Nginx镜像(其实此命令就等同于`docker pull nginx:latest`)

- `docker pull nginx:xxx`:下载指定版本的Nginx镜像(xxx指具体版本号)

# 检查镜像

`docker images`

# 创建网络

如果没有指定网络,可以先创建网络:`docker network create net`

# 创建Nginx目录

1. 在root目录下创建Nginx目录

`cd /root`

`mkdir nginx`

2. 在nginx目录下创建文件夹log、conf、html

**log、conf、html分别对应nginx的日志文件、配置文件、静态内容目录**

`cd nginx`

`mkdir log conf html`

3. 将容器中的配置文件、数据文件、日志文件、初始化文件复制到主机的nginx目录下

- 启动临时容器:

`docker run --name nginx -p 80:80 -d nginx`

- 将临时容器nginx.conf文件复制到宿主机:

`docker cp nginx:/etc/nginx/nginx.conf /root/nginx/conf/nginx.conf`

- 将临时容器conf.d文件夹下内容复制到宿主机:

`docker cp nginx:/etc/nginx/conf.d /root/nginx/conf/conf.d`

- 将临时容器中的html文件夹复制到宿主机:

`docker cp nginx:/usr/share/nginx/html /root/nginx/`

- 关闭临时容器

`docker stop nginx`

- 删除临时容器

`docker rm nginx`

# 创建容器并运行

```cmd
docker run \
  -d \
  --name nginx \
  -p 80:80 \
  -v /root/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
  -v /root/nginx/conf/conf.d:/etc/nginx/conf.d \
  -v /root/nginx/log:/var/log/nginx \
  -v /root/nginx/html:/usr/share/nginx/html \
  --network net \
  nginx
```

解释:
- `docker run`:运行容器
- `-d`:后台运行容器
- `--name nginx`:指定容器名称为nginx
- `-p 80:80`:将容器的80端口映射到主机的80端口
- `-v /root/nginx/conf/nginx.conf:/etc/nginx/nginx.conf`:将主机的/root/nginx/conf/nginx.conf文件映射到容器的/etc/nginx/nginx.conf文件
- `-v /root/nginx/conf/conf.d:/etc/nginx/conf.d`:将主机的/root/nginx/conf/conf.d目录映射到容器的/etc/nginx/conf.d目录
- `-v /root/nginx/log:/var/log/nginx`:将主机的/root/nginx/log目录映射到容器的/var/log/nginx目录
- `-v /root/nginx/html:/usr/share/nginx/html`:将主机的/root/nginx/html目录映射到容器的/usr/share/nginx/html目录
- `--network net`:指定容器的网络为net
- `nginx`:指定镜像名称为nginx

# 检查nginx运行状态

- `docker ps`:检查容器是否运行
- `docker inspect nginx`:查看容器详细信息

确认nginx运行之后,即可在浏览器中输入`http://192.168.149.100:80`(其中`192.168.149.100`为nginx所在主机的ip地址,需替换成自己的)