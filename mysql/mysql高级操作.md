## 使用datagrip创建数据库
```
CREATE DATABASE `test_db` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;  
```
## 初始化数据库需要查询关键信息
```
select Host,User,authentication_string,password_expired from user;
```
## 登录mysql
```
cd /data/zouy/install/mysql
cd /data/zouy/install/mysql-s
bin/mysql --socket=/data/zouy/install/mysql/mysql.sock -u root -p
```
## 备份整个库
```
#!/bin/bash
#BASE_DIR
BACK_DIR=/data/zouy/backup
BACK_LOG_DIR=/data/zouy/backup/log
#数据库参数
account=root
pwd=***
mysqlsock=/data/zouy/install/mysql/mysql.sock
date=`date '+%Y-%m-%d_%H%M%S'`
db=pocket
cd /data/zouy/install/mysql
bin/mysqldump --socket=${mysqlsock} -u${account} -p${pwd} ${db} | gzip > ${BACK_DIR}/${db}_backup_${date}.sql.gz 2>${BACK_LOG_DIR}/${db}_backlog_${date}
echo "backup success!"
```
## 备份指定表
```
#!/bin/bash
#接收多个参数，控制备份具体的表
tbList=" "
for i in $@
do
tbList=${tbList}" $i "
done
echo "database & tables: "${tbList}
#BASE_DIR
DUMP_DIR=/data/zouy/backup/dump
DUMP_LOG_DIR=/data/zouy/backup/log
#数据库参数
account=root
pwd=***
mysqlsock=/data/zouy/install/mysql/mysql.sock
date=`date '+%Y-%m-%d_%H%M%S'`
cd /data/zouy/install/mysql
bin/mysqldump --socket=${mysqlsock} -u${account} -p${pwd} ${tbList} | gzip > ${DUMP_DIR}/mysql-pocket_tables_${date}.sql.gz 2>${DUMP_LOG_DIR}/$1_dumplog_${date}
echo "dump success!"
#将备份文件发送到远程服务器
cd ${DUMP_DIR}
scp mysql-pocket_tables_${date}.sql.gz root@118.126.96.78:${DUMP_DIR} 2>>${DUMP_LOG_DIR}/$1_dumplog_${date}
echo "scp success!"
```
## 备份存储过程
```
#!/bin/bash
#BASE_DIR
PROCEDURE_DIR=/data/zouy/backup/procedure
PROCEDURE_LOG_DIR=/data/zouy/backup/log
#数据库参数
account=root
pwd=***
mysqlsock=/data/zouy/install/mysql/mysql.sock
date=`date '+%Y-%m-%d_%H%M%S'`
db=pocket
cd /data/zouy/install/mysql
bin/mysqldump --socket=${mysqlsock} -u${account} -p${pwd} -n -d -t -R ${db} | gzip > ${PROCEDURE_DIR}/${db}_procedure_${date}.sql.gz 2>${PROCEDURE_LOG_DIR}/${db}_procedurelog_${date}
echo "backup success!"
```
## 恢复
执行sql语句而已，主从同步中，从库崩了后很常见的操作。以下命令表示在pocket数据库中执行mysql-pocket_tables.sql脚本。
```
#!/bin/bash
cd /data/zouy/install/mysql
bin/mysql --socket=/data/zouy/install/mysql/mysql.sock -uroot -p*** -Dpocket < /data/zouy/backup/mysql-pocket_tables.sql
```