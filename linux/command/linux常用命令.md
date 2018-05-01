## 网络
> apt install net-tools       # ifconfig 
apt install iputils-ping     # ping
netstat -ant | grep 4000     #查看端口占用

<!--more-->

## 进程管理
> ps aux | grep nginx
ps -le | more
pstree -pu
top
kill -l
kill -HUP (pid)  #平滑重启
kill -1 (pid)  #平滑重启
kill -KILL (pid)  #强制杀死进程
kill -9 (pid)  #强制杀死进程
killall -9 httpd
w #当前登录的用户
pkill -9 -t tty1 #按照终端号踢出用户

## 工作(后台)管理
> jobs -l #显示工作的pid
(command) & #放在后台执行
ctrl+z #放在后台暂停
find / -name tomasyao &
/usr/local/mysql/bin/mysqld --user=mysql & #d就是daemon守护进程
后台命令脱离登录终端执行：
1. /etc/rc.local
2. 系统定时任务
3. nohup

## 系统资源查看
> vmstat 1 3 #[刷新延迟，刷新次数]
dmesg #内核自启信息
dmesg | grep CPU
free #内存使用状态
cat /proc/cpuinfo
uptime
uname -a
file /bin/ls #判断当前系统的位数
lsb_release -a #发行版
lsof /sbin/init

## 系统定时任务
> at now +2 minutes
atq
atrm
crontab -e(编辑)
\*\*\*\*\* 执行的任务
1. \* 一小时当中的第几分钟 0-59
2. \* 一天当中的第几小时 0-23
3. \* 一个月当中的第几天 1-31
4. \* 一年当中的第几月 1-12
5. \* 一周当中的星期几 0-7(0-7都代表星期日)
特殊符号
**\*** 任何时间(第一个\*代表一小时每分钟执行一次)
**,** 不连续时间 0 8,12,16 \* \* \* 命令(每天8点0分，12点0分，16点0分执行)
**-** 连续时间范围 0 5 \* \* 1-6 命令(周一到周六的凌晨5点0分执行)
**\*/n** 间隔执行 \*/10 \* \* \* \* 命令(每隔10分钟执行) 
crontab -l
crontab -r
/etc/crontab


## 目录文件权限
> 文件 最高权限 x
目录 最高权限 w
    0 5(rx) 7(rwx)
    4 1 6
useradd user1
passwd user1
chmod
chown #修改文件的所有者
chgrp #修改文件的所有组
groupadd user
gpasswd -a user1 user #将user1加到user组
chown user1:user dir
umask
文件默认不能建立为执行文件，必须手工赋予执行权限
所以文件默认权限最大为666
默认权限需要换算成字母再相减
建立文件之后的默认权限，为666减去umask
目录默认权限最大为777
永久修改
vi /etc/profile