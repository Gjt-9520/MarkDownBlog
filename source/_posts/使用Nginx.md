---
title: "使用Nginx"
date: 2024-08-24
description: ""
cover: https://github.com/Gjt-9520/MarkDownBlog/blob/main/source/coverImages/Bimage-135/Bimage115.jpg?raw=true
tags: ["Nginx"]
category: "实用"
updated: 2024-08-25
  
top_group_index: 
---

# Nginx

Nginx是开源、高性能、高可靠的Web和反向代理服务器,而且支持热部署,几乎可以做到7×24小时不间断运行,即使运行几个月也不需要重新启动,还能在不间断服务的情况下对软件版本进行热更新

性能是Nginx最重要的考量,其占用内存少、并发能力强、能支持高达5w个并发连接数,最重要的是,Nginx是免费的并可以商业化,配置使用也比较简单

# 应用场景

1. **反向代理服务器**:Nginx可以作为反向代理服务器,接收来自客户端的请求,然后将这些请求转发给后端的服务器,例如,可以将Ngix设置为一个负载均衡器,将客户端请求均衡地分发到多台Web服务器上

2. **负载均衡**:Ngiⅸ支持多种负载均衡算法,如轮询、最小连接数、IP哈希等,通过这些算法,Ngix可以有效地将流量分发到多个后端服务器,提升整个系统的可用性和性能

3. **静态文件处理**:Ngix处理静态文件的效率非常高,比如HTML、CSS、JavaScript、图片、视频等,对于静态资源,它能够直接从磁盘读取并返回客户端,而不需要转发到后端应用服务器

4. **动静分离**:在实际应用中,经常将动态请求和静态请求分离,动态请求交由应用服务器处理,静态请求交由Ngⅸ处理,这可以显著提升整体性能,Ngiⅸ能够通过正则表达式或其他规则将请求路由到不同的后端服务器或文件路径

5. **日志记录与监控**:Ngⅸ提供丰富的日志记录功能,包括访问日志和错误日志,通过这些日志,能够监控服务器的状态、流量和性能,及时发现和解决潜在问题

6. **安全与访问控制**:Ngix支持通过SSL/TLS实现HTTPS加密,保证数据传输的安全性,同时,它还支持IP黑白名单、基于HTTP头的控制等多种访问控制方式

7. **缓存功能**:Ngiⅸ具备强大的缓存功能,例如代理缓存和FastCGI缓存,可以显著提升后端响应速度,减轻服务器负载

8. **模块化设计**:Ngiⅸ的模块化设计使其具备高度的可扩展性,通过加载不同的模块,用户可以为Ngix增加不同的功能,如性能监控、限流限速、防盗链等

# 常用命令

要启动nginx,需要运行可执行文件,nginx启动之后,可以通过调用可执行文件附带`-s`参数 来控制它,使用以下语法:

- `nginx.exe -s stop`:立即关闭

- `nginx.exe -s quit`:正常关闭

- `nginx.exe -s reload`:重新加载配置文件

- `nginx.exe -s reopen`:重新打开日志文件

# 正向代理和反向代理

正向代理和反向代理都是代理服务器的两种应用场景,它们在网络请求的处理过程中扮演不同的角色:

- **正向代理(Forward Proxy)**:正向代理位于客户端和目标服务器之间,客户端通过正向代理来访问目标服务器
    正向代理代表客户端发起请求,隐藏客户端的真实身份
    常见的应用场景包括使用魔法、访问内网资源、缓存和过滤等

- **反向代理(Reverse Proxy)**:反向代理位于客户端和目标服务器之间,客户端直接访问反向代理服务器,反向代理将请求转发给目标服务器
    反向代理代表目标服务器接收请求,隐藏目标服务器的真实身份
    常见的应用场景包括负载均衡、安全防护、缓存和SSL终端等

# 配置

nginx.conf文件:

```yaml
#user  nobody; # 指定nginx运行用户
worker_processes  1; # 设置Nginx的工作进程数为1,通常可以设置为CPU核心数

#error_log  logs/error.log; # 默认日志级别为warn
#error_log  logs/error.log  notice; # 默认日志级别为info
#error_log  logs/error.log  info; # 默认日志级别为notice

#pid        logs/nginx.pid; # 指定nginx进程pid文件


events {
    worker_connections  1024; # 指定每个工作进程可以同时打开的最大连接数
}


http {
    include       mime.types; # 引入mime.types文件
    default_type  application/octet-stream; # 默认文件类型
    client_max_body_size 100M; # 设置客户端请求体最大值
    client_body_buffer_size 128k; # 设置请求体缓存区大小
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main; # 设置访问日志

    sendfile        on; # 开启sendfile功能以优化文件传输性能
    #tcp_nopush     on; # 开启tcp_nopush功能以优化网络性能

    #keepalive_timeout  0; # 关闭keepalive功能
    keepalive_timeout  65; # 设置连接超时时间为65秒

    #gzip  on; # 开启gzip压缩

# 文件服务
upstream fileserver{
        server 192.168.101.65:9000 weight=10; 
}

# 后台网关
upstream gatewayserver{
        server 127.0.0.1:63010 weight=10; 
} 

server {
        listen       80; # 监听80端口
        server_name  www.51xuecheng.cn localhost; # 虚拟主机域名
        #rewrite ^(.*) https://$server_name$1 permanent;
        #charset koi8-r;
        ssi on;
        ssi_silent_errors on;
        #access_log  logs/host.access.log  main;

        location / {
            alias   D:\Project\XuechengPlus(SpringBoot)\xc-ui-pc-static-portal/;
            index  index.html index.htm;
        }

        # api
        location /api/ {
                proxy_pass http://gatewayserver/;
        } 

        # openapi
        location /open/content/ {
                proxy_pass http://gatewayserver/content/open/;
        } 
        location /open/media/ {
                proxy_pass http://gatewayserver/media/open/;
        } 
        location /course/ {
        proxy_pass http://fileserver/mediafiles/course/;
        }
        
        # 静态资源
        location /static/img/ {  
                alias  D:\Project\XuechengPlus(SpringBoot)\xc-ui-pc-static-portal/img/;
        } 
        location /static/css/ {  
                alias   D:\Project\XuechengPlus(SpringBoot)\xc-ui-pc-static-portal/css/;
        } 
        location /static/js/ {  
                alias   D:\Project\XuechengPlus(SpringBoot)\xc-ui-pc-static-portal/js/;
        } 
        location /static/plugins/ {  
                alias   D:\Project\XuechengPlus(SpringBoot)\xc-ui-pc-static-portal/plugins/;
                add_header Access-Control-Allow-Origin http://ucenter.51xuecheng.cn;  
                add_header Access-Control-Allow-Credentials true;  
                add_header Access-Control-Allow-Methods GET;
        } 
        location /plugins/ {  
                alias   D:\Project\XuechengPlus(SpringBoot)\xc-ui-pc-static-portal/plugins/;
        } 
        location /course/preview/learning.html {
                alias D:\Project\XuechengPlus(SpringBoot)\xc-ui-pc-static-portal/course/learning.html;
        } 
        location /course/search.html {  
                root   D:\Project\XuechengPlus(SpringBoot)\xc-ui-pc-static-portal;
        } 
        location /course/learning.html {  
                root   D:\Project\XuechengPlus(SpringBoot)\xc-ui-pc-static-portal;
        } 

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html; # 错误页面
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }

# 文件服务
server {
        listen       80;
        server_name  file.51xuecheng.cn;
        #charset koi8-r;
        ssi on;
        ssi_silent_errors on;
        #access_log  logs/host.access.log  main;
        location /video {
            proxy_pass   http://fileserver;
        }
        
        location /mediafiles {
            proxy_pass   http://fileserver;
        }
   }
}
```