---
title: "安装ElasticSearch、Kibana"
date: 2024-07-28
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage86.jpg?raw=true
tags: ["Docker","ElasticSearch","Kibana"]
category: "配置"
updated: 2024-07-29
  
top_group_index: 
---

# 下载镜像

- [Docker Hub官方网站](https://hub.docker.com/)

- `docker pull elasticsearch`:下载最新版ElasticSearch镜像

- `docker pull kibana`:下载最新版Kibana镜像

# 检查镜像

`docker images`

# 创建网络

如果没有指定网络,可以先创建网络:`docker network create net`

# 创建容器并运行

## ElasticSearch

```cmd
docker run \
  -d \
  --name es \
  -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" \
  -e "discovery.type=single-node" \
  -v es-data:/usr/share/elasticsearch/data \
  -v es-plugins:/usr/share/elasticsearch/plugins \
  --privileged \
  --network net \
  -p 9200:9200 \
  -p 9300:9300 \
  elasticsearch:7.12.1
```

解释:
- `docker run`:运行容器
- `-d`:后台运行容器
- `--name es`:指定容器名称为es
- `-e "ES_JAVA_OPTS=-Xms512m -Xmx512m"`:设置Elasticsearch的Java选项,指定JVM的最小和最大内存为512MB
- `-e "discovery.type=single-node"`:设置Elasticsearch的节点类型为单节点
- `-v es-data:/usr/share/elasticsearch/data`:将主机的es-data目录挂载到容器的/usr/share/elasticsearch/data目录,用于存储Elasticsearch的数据
- `-v es-plugins:/usr/share/elasticsearch/plugins`:将主机的es-plugins目录挂载到容器的/usr/share/elasticsearch/plugins目录,用于存储Elasticsearch的插件
- `--privileged`:给予容器特权,使其能够访问主机上的设备
- `--network net`:指定容器的网络为net
- `-p 9200:9200`:将主机的9200端口映射到容器的9200端口,用于访问Elasticsearch的HTTP API
- `-p 9300:9300`:将主机的9300端口映射到容器的9300端口,用于访问Elasticsearch的TCP API
- `elasticsearch:7.12.1`:指定镜像名称为elasticsearch

## Kibana

```cmd
docker run \
  -d \
  --name kibana \
  -e ELASTICSEARCH_HOSTS=http://es:9200 \
  --network net \
  -p 5601:5601  \
  kibana:7.12.1
```

解释:
- `-d`:后台运行容器
- `--name kibana`:指定容器名称为kibana
- `-e ELASTICSEARCH_HOSTS=http://es:9200`:设置Kibana与Elasticsearch的连接地址
- `--network net`:指定容器的网络为net
- `-p 5601:5601`:将容器的5601端口映射到主机的5601端口
- `kibana:7.12.1`:指定镜像名称为kibana

# 检查运行状态

- `docker ps`:检查容器是否运行
- `docker inspect es`和`docker inspect kibana`:查看容器详细信息

确认es运行之后,即可在浏览器中输入`http://192.168.149.100:9300`(其中`192.168.149.100`为es所在主机的ip地址,需替换成自己的)

确认kibana运行之后,即可在浏览器中输入`http://192.168.149.100:5601`(其中`192.168.149.100`为kibana所在主机的ip地址,需替换成自己的)

# 安装IK分词器插件

1. 查看es的插件目录对应的数据卷

`docker volume ls`

2. 进入es的插件目录

`cd /var/lib/docker/volumes/es-plugins/_data`

3. 下载DelayExchange插件到挂载的插件目录下

[Github插件地址](https://github.com/infinilabs/analysis-ik)

4. 重启es,加载插件

`docker restart es`