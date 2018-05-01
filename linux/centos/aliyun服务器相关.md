---
title: aliyun服务器相关
date: 2018-01-25 10:25:32
tags: [aliyun,服务器]
---
**阿里云服务器centos7 64位 必备软件**

> ### java

* **安装(版本1.8)**
* **配置环境变量**
```bash
export JAVA_HOME=/home/zouy/Downloads/anzhuangbao/java/jdk1.8.0_121
export JAVA_BIN=$JAVA_HOME/bin
export JAVA_LIB=$JAVA_HOME/lib
export CLASSPATH=.:$JAVA_LIB/tools.jar:$JAVA_LIB/dt.jar:$CLASSPATH
export PATH=$PATH:$JAVA_BIN
```

<!--more-->

> ### tomcat

* **安装(版本8)**
* **配置服务与自启动**
1. centos7采用systemctl命令代替service和chkconfig,方便了服务的管理与程序的开机自启
```bash
cd /usr/lib/systemd/system
```
2. 编写tomcat.service服务文件
```bash
[Unit]
Description=Tomcat
After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
#java环境变量,视具体情况而定
Enviroment="JAVA_HOME=/data/zouy/install/jdk1.8.0_144"

PIDFile=/data/zouy/install/tomcat/tomcat.pid
ExecStart=/data/zouy/install/tomcat/bin/startup.sh
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```
3. 配置开机自启
```bash
#开机自启
systemctl enable tomcat
#启动tomcat
systemctl start tomcat
#查看状态
systemctl status tomcat
```

> ### nginx

* **安装(版本1.12.1)**
* **配置环境变量**
```bash
export PATH=$PATH:/usr/local/nginx/sbin
```
* **配置文件(/usr/local/nginx/conf/nginx.conf)**
```bash
worker_processes  1;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;

    keepalive_timeout  65;

    client_max_body_size 20m;

    upstream tomcat_server{
            server localhost:8080;
    }

    server {
        listen       80;
        server_name  localhost;

        location / {
            proxy_pass http://tomcat_server;

            if ($request_method = 'OPTIONS') {  
                add_header 'Access-Control-Allow-Origin' '*';  
                add_header 'Access-Control-Allow-Credentials' 'true';  
                add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';  
                add_header 'Access-Control-Allow-Headers' 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X  -Requested-With,If-Modified-Since,Cache-Control,Content-Type';  
                #add_header 'Access-Control-Max-Age' 1728000;  
                add_header 'Content-Type' 'text/plain charset=UTF-8';  
                add_header 'Content-Length' 0;  
                return 200;  
            } 
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```
* **配置服务与自启动**
1. centos7采用systemctl命令代替service和chkconfig,方便了服务的管理与程序的开机自启
```bash
cd /usr/lib/systemd/system
```
2. 编写nginx.service服务文件
```bash
[Unit]
Description=Nginx
After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
Type=forking

#PIDFile=/data/zouy/install/nginx/nginx.pid
ExecStart=/usr/local/nginx/sbin/nginx
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```
3. 配置开机自启
```bash
#开机自启
systemctl enable nginx
#启动nginx
systemctl start nginx
#查看状态
systemctl status nginx
```

> ### mysql

* **安装并初始化(版本5.7.18)**
* **配置服务与自启动**
1. centos7采用systemctl命令代替service和chkconfig,方便了服务的管理与程序的开机自启
```bash
cd /usr/lib/systemd/system
```
2. 编写mysql.service服务文件
```bash
[Unit]
Description=Mysql
After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
Type=forking

#PIDFile=/data/zouy/install/mysql/mysql-files/mysql.pid
ExecStart=/data/zouy/install/mysql/support-files/mysql.server start
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=false

[Install]
WantedBy=multi-user.target
```
3. 配置开机自启
```bash
#开机自启
systemctl enable mysql
#启动mysql
systemctl start mysql
#查看状态
systemctl status mysql
```

> ### redis

* **安装(版本3.2.10)**
* **配置文件(/data/zouy/install/redis/redis.conf)**
```bash
#只挑重点
port 6379
pidfile /var/run/redis_6379.pid
logfile /data/zouy/install/redis/log/redis.log
dbfilename dump.rdb
dir /data/zouy/install/redis/data
#密码
requirepass *****
```
* **配置服务与自启动**
1. centos7采用systemctl命令代替service和chkconfig,方便了服务的管理与程序的开机自启
```bash
cd /usr/lib/systemd/system
```
2. 编写redis.service服务文件
```bash
[Unit]
Description=Redis
After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
Type=forking

ExecStart=/data/zouy/install/redis/bin/redis-server /data/zouy/install/redis/redis.conf         
ExecReload=/bin/kill -USR2 $MAINPID
ExecStop=/bin/kill -SIGINT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```
3. 配置开机自启
```bash
#开机自启
systemctl enable redis
#启动redis
systemctl start redis
#查看状态
systemctl status redis
```


