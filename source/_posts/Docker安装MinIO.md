---
title: "安装MinIO"
date: 2024-08-15
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage104.jpg?raw=true
tags: ["Docker", "MinIO"]
category: "配置"
updated: 2024-08-16
  
top_group_index: 
---

# 下载镜像       

- [Docker Hub官方网站](https://hub.docker.com/)

- `docker pull minio/minio`:下载最新版MinIO镜像(其实此命令就等同于`docker pull minio/minio:latest`)

- `docker pull minio/minio:xxx`:下载指定版本的MinIO镜像(xxx指具体版本号)

# 检查镜像

`docker images`

# 创建minio目录

1. 在root目录下创建minio目录

`cd /root`

`mkdir minio`

2. 在minio目录下创建文件夹conf、data

**conf、data分别对应minio的配置文件、储存文件**

`cd minio`

`mkdir conf data`

# 创建容器并运行

```cmd
docker run \
  -d \
  --name minio \
  -p 9000:9000  \
  -p 9001:9001  \
  -e "MINIO_ROOT_USER=minioadmin" \
  -e "MINIO_ROOT_PASSWORD=minioadmin" \
  -v /root/minio/data:/data \
  -v /root/minio/conf:/root/.minio \
  minio/minio server  /data --console-address ":9001" --address ":9000"
```

解释:
- `docker run`:运行容器
- `-d`:后台运行容器
- `--name minio`:指定容器名称为minio
- `-p 9000:9000`:将容器的9000端口映射到主机的9000端口
- `-p 9001:9001`:将容器的9001端口映射到主机的9001端口
- `-e "MINIO_ROOT_USER=minioadmin"`:设置MinIO的用户名为minioadmin
- `-e "MINIO_ROOT_PASSWORD=minioadmin"`:设置MinIO的密码为minioadmin
- `-v /root/minio/data:/data`:将主机的/root/minio/data目录挂载到容器的/data目录
- `-v /root/minio/conf:/root/.minio`:将主机的/root/minio/conf目录挂载到容器的/root/.minio目录
- `minio/minio server  /data --console-address ":9001" --address ":9000"`:启动minio容器,并指定数据目录为/data,控制台端口为9001,服务端口为9000

# 检查MinIO运行状态

- `docker ps`:检查容器是否运行
- `docker inspect minio`:查看容器详细信息

确认MinIO运行之后,即可在浏览器中输入`http://192.168.149.100:9001`(其中`192.168.149.100`为MinIO所在主机的ip地址,需替换成自己的)

访问MinIO的控制台,用户名和密码都是minioadmin