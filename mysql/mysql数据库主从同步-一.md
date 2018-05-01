---
title: mysql数据库主从同步(一)
date: 2017-07-15 19:08:11
tags: [mysql,主从同步]
---
## 安装第二个mysql
**1.主库my.cnf配置**
>在上一篇文章中已经安装了一个mysql作为主库，要想顺利安装另一个mysql作为从库可不是那么容易。主库的有些配置需要先做相应修改。

/etc/init.d/mysqld启动脚本读取my.cnf配置文件有先后顺序，最先读到的是/etc/my.cnf,但是用到的却是最后读到的。单个数据库都好说，数据库多了为了方便管理我们不把my.cnf放在/etc/目录下，而是放在$basedir/目录下，也就是数据库解压安装生成的目录下。

**于是:**
```
sudo cp /etc/my.cnf /home/zouy/Downloads/mysql/mysql-5.7.18
sudo rm /etc/my.cnf
```
<!--more-->

然后对my.cnf的内容做一下补充
> [client]
port = 3306
default-character-set = utf8
[mysqld]
port = 3306
basedir = /home/zouy/Downloads/mysql/mysql-5.7.18
datadir = /home/zouy/Downloads/mysql/mysql-5.7.18/mysql-files
socket = /home/zouy/Downloads/mysql/mysql-5.7.18/mysql.sock
#跳过权限验证
#skip-grant-tables
[mysqld]
max_connections = 1000
max_connect_errors = 1000
sort_buffer_size = 16M
query_cache_size = 64M
query_cache_limit = 4M
#default-storage-engine=INNODB
[mysqldump]
max_allowed_packet = 32M

`实际的my.cnf配置项有很多，以后再具体分析`

**2.主库mysqld服务配置**
这里只是将上一篇没有说到的列出来
> basedir=/home/zouy/Downloads/mysql/mysql-5.7.18
datadir=/home/zouy/Downloads/mysql/mysql-5.7.18/mysql-files
#这里先这样改，好像暂时没什么用,默认是/var/lock/subsys会出奇怪的问题。
lockdir='/home/zouy/Downloads/mysql/mysql-5.7.18/var/lock/subsys'

**3.从库解压安装**
基本与主库解压安装类似，对上篇文章的安装细微优化后，这里我简单列出安装命令
```
cd /home/zouy/Downloads/mysql
#上一篇已经重命名过所以不用担心解压有问题
tar -zxvf mysql-5.7.18-linux-glibc2.5-x86_64.tar.gz
mv mysql-5.7.18-linux-glibc2.5-x86_64 mysql-5.7.18-slave 
cd /home/zouy/Downloads/mysql/mysql-5.7.18-slave 
mkdir mysql-files

#zouy为我的Ubuntu系统的登录用户
sudo bin/mysqld --initialize --user=zouy --basedir=/home/zouy/Downloads/mysql/mysql-5.7.18-slave --datadir=/home/zouy/Downloads/mysql/mysql-5.7.18-slave/mysql-files

sudo cp support-files/mysql.server /etc/init.d/mysqld-slave
```

`环境变量就不用配了,之前为mysql配过也建议取消掉`

**4.从库my.cnf配置**
从主库里考一份然后改一下相关变量即可

```
cp /home/zouy/Downloads/mysql/mysql-5.7.18-slave/my.cnf  /home/zouy/Downloads/mysql/mysql-5.7.18-slave 
```

> [client]
port = 3307
default-character-set = utf8
[mysqld]
port = 3307
basedir = /home/zouy/Downloads/mysql/mysql-5.7.18-slave
datadir = /home/zouy/Downloads/mysql/mysql-5.7.18-slave/mysql-files
socket = /home/zouy/Downloads/mysql/mysql-5.7.18-slave/mysql.sock
#跳过权限验证
#skip-grant-tables
[mysqld]
max_connections = 1000
max_connect_errors = 1000
sort_buffer_size = 16M
query_cache_size = 64M
query_cache_limit = 4M
#default-storage-engine=INNODB
[mysqldump]
max_allowed_packet = 32M

**5.从库mysqld-slave服务配置**
从主库里考一份然后改一下相关变量即可

```
sudo cp /etc/init.d/mysqld /etc/init.d/mysqld-slave
```

> basedir=/home/zouy/Downloads/mysql/mysql-5.7.18-slave
datadir=/home/zouy/Downloads/mysql/mysql-5.7.18-slave/mysql-files
#这里先这样改，好像暂时没什么用,默认是/var/lock/subsys会出奇怪的问题。
lockdir=’/home/zouy/Downloads/mysql/mysql-5.7.18-slave/var/lock/subsys’

**6.从库账户配置**
这里只说明本地通过$basedir/bin/mysql登录数据库服务器需要指定--socket参数，其余对账户的权限分配以及配置远程登录请参照上一篇文章。

```
cd /home/zouy/Downloads/mysql/mysql-5.7.18-slave
bin/mysql --socket=/home/zouy/Downloads/mysql/mysql-5.7.18-slave/mysql.sock -u root -p
......
```

`注意，依然先跳过访问控制，改变root账户的密码以及过期时间后，重新通过密码登录mysql服务器。`

**7.小结**
> 以后通过如下命令来管理主从数据库
```
    service mysqld [start|stop|restart|status]
    service mysqld-slave [start|stop|restart|status]
```
