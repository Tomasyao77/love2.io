## 配置主库
**1.my.cnf**
> 修改主库my.cnf配置文件

```
[client]
port = 3306
default-character-set = utf8
[mysqld]
port = 3306
basedir = /home/zouy/Downloads/mysql/mysql-5.7.18
datadir = /home/zouy/Downloads/mysql/mysql-5.7.18/mysql-files
socket = /home/zouy/Downloads/mysql/mysql-5.7.18/mysql.sock
#主从同步 主数据库
server-id=1   #必须。设置服务器id，为1表示主服务器。规范为服务器IP的后段
log_bin=mysql-bin  #必须。启动MySQ二进制日志系统。
binlog-do-db=test  #需要同步的数据库名，如果有多个数据库，可重复此参数，每个数据库一行。
binlog-ignore-db=mysql   #不同步mysql系统数据库。
```

`test为数据库名`

**2.主库同步点**
<!--more-->
> 重启主库后，本地使用mysql客户端通过socket文件连接到主库

```
$ service mysqld restart
$ cd /home/zouy/Downloads/mysql/mysql-5.7.18
$ bin/mysql --socket=/home/zouy/Downloads/mysql/mysql-5.7.18/mysql.sock -u admin -p
......(输入密码后进入mysql)
mysql>show master status;
```

`admin为我自己建立的一个用户，改成自己的`

可以看到主库的相关信息：
![master](http://119.29.141.240/group1/M00/00/00/Cmh6v1lspAuAN4qEAACwaD7ANJU973.png)

**3.在主库中为从库建立有权复进行制的用户**
```
mysql>grant replication slave on *.* to 'slave'@'%' identified by 'slave'
```

## 配置从库
**1.my.cnf**
> 修改从库my.cnf配置文件

```
[client]
port = 3307
default-character-set = utf8
[mysqld]
port = 3307
basedir = /home/zouy/Downloads/mysql/mysql-5.7.18-slave
datadir = /home/zouy/Downloads/mysql/mysql-5.7.18-slave/mysql-files
socket = /home/zouy/Downloads/mysql/mysql-5.7.18-slave/mysql.sock
#主从同步 从数据库
server-id=2   #必须。设置服务器id，为2表示从服务器。规范为服务器IP的后段
#log_bin=mysql-bin  #不必须。启动MySQ二进制日志系统。
binlog-do-db=test  #需要同步的数据库名，如果有多个数据库，可重复此参数，每个数据库一行。
binlog-ignore-db=mysql   #不同步mysql系统数据库。
relay_log         = mysql-relay-bin
log_slave_updates = 1
read_only         = 1
```

`test为数据库名`

**2.重启从库**
> 重启从库后，本地使用mysql客户端通过socket文件连接到主库

```
$ service mysqld-salve restart
$ cd /home/zouy/Downloads/mysql/mysql-5.7.18-salve
$ bin/mysql --socket=/home/zouy/Downloads/mysql/mysql-5.7.18-salve/mysql.sock -u admin -p
......(输入密码后进入mysql)
mysql>show master status;
```

`admin为我自己建立的一个用户，改成自己的`

**3.从库启动同步**
```
mysql>change master to master_host='10.139.9.112',
    ->master_user='slave',
    ->master_password='slave',
    ->master_log_file='mysql-bin.000002',
    ->master_log_pos=154；
mysql>start slave;
mysql>show slave status\G;
```

> 其中master_log_file和master_log_pos为之前在主库中登录进入mysql后使用show master status命令查出来的结果。

然后可以看到从库的对主库的同步情况

![slave](http://119.29.141.240/group1/M00/00/00/Cmh6v1lsqQ6AYsOJAAF9Oqj1mgI877.png)

`可以看到Slave_IO_Running和Slave_SQL_Running都为Yes，表示同步成功。之后在主库中修改test库里的表，可以发现从库也会相应变化。`