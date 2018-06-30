### 网络
> apt install net-tools       # ifconfig 
apt install iputils-ping     # ping
netstat -ant | grep 4000     #查看端口占用

### 防火墙
> vi /etc/sysconfig/iptables
-A RH-Firewall-1-INPUT -p tcp -m state --state NEW -m tcp --dport 8080 -j ACCEPT
`以上在iptables里面添加信任端口。`
service alfa_iptables status
service alfa_iptables restart

### 进程管理
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

### 工作(后台)管理
> jobs -l #显示工作的pid
(command) & #放在后台执行
ctrl+z #放在后台暂停
find / -name tomasyao &
/usr/local/mysql/bin/mysqld --user=mysql & #d就是daemon守护进程
后台命令脱离登录终端执行：
1. /etc/rc.local
2. 系统定时任务
3. nohup

### 系统资源查看
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

### 系统定时任务
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


### 目录文件权限
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


### 磁盘
> 使用df -h 命令查看磁盘剩余空间
用du -ms /usr命令查看常用的usr目录大小 以M为单位 summarize
用find . -size +100M 命令找到大文件

### fcitx输入法管理
> fcitx-config-gtk3 打开fcitx配置至少添加两种输入法(搜狗和english)才能按shift进行中英切换!!!

### centos7服务
> centos7 用systemctl代替chkconfig和service命令,且.service服务脚本放在/usr/lib/systemd/system目录
我们一般需配置tomcat mysql nginx redis等程序的服务文件

### redis
> redis: redis-cli;auth "6379D9Lab39108180232"获取权限

### hadoop
> wget http://mirror.bit.edu.cn/apache/hadoop/common/hadoop-1.2.1/hadoop-1.2.1.tar.gz
下载安装hadoop。
mv hadoop-2.7.1.tar.gz /opt/
cd /opt/
ls
tar -zxvf hadoop-2.7.2 tar.gz
配置文件
1.hadoop-env.sh 外部环境
修改JAVA_HOME
2.core-site.xml 工作目录
hadoop.tmp.dir dfs.name.dir fs.default.name
3.hdfs-site.xml 数据存放目录
dfs.data.dir
4.mapred-site.xml 任务调度器
mapred.job.tracker
5./etc/proifle 配置环境变量
HADOOP_HOME PATH

### ubuntu
> gksu nautilus（图形化root权限）
vi快速删除文本内容 :%d

### sublime text3
> sublime text3 输入中文的解决方法
1. 下载我们需要的文件,打开终端 ,输入：
git clone https://github.com/lyfeyaj/sublime-text-imfix.git
2. 将下载的文件解压之后，移到当前目录（～目录下边），然后执行下边命令：
cd ~/sublime-text-imfix （前提：解压后的sublime-text-imfix必须在~目录下） 
sudo cp ./lib/libsublime-imfix.so /opt/sublime_text/ 
sudo cp ./src/subl /usr/bin/
3. 最后把sublime都关掉，然后在终端输入subl，就可以在sublime使用中文了(*必须在终端输入subl启动sublime才起作用的)
4.为了让通过桌面图标启动的sublime也能输入中文，只需编辑.desktop文件即可，如下所示：
命令
sudo gedit /usr/share/applications/sublime_text.desktop
将[Desktop Entry]中的字符串
Exec=/opt/sublime_text/sublime_text %F
修改为
Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text %F"
将[Desktop Action Window]中的字符串
Exec=/opt/sublime_text/sublime_text -n
修改为
Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text -n"
将[Desktop Action Document]中的字符串
Exec=/opt/sublime_text/sublime_text --command new_file
修改为
Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text --command new_file"
注意：
修改时请注意双引号"",否则会导致不能打开带有空格文件名的文件。
此处仅修改了/usr/share/applications/sublime-text.desktop，但可以正常使用了。
opt/sublime_text/目录下的sublime-text.desktop可以修改，也可不修改。

### 压缩解压
> #zip
unzip [name]
zip -r [name] [dir]
#.tar.gz
tar -zxvf [name]
tar -zcvf [name] [dir]
#.tar.bz2
tar -jxvf [name]
tar -jcvf [name] [dir]
#解压缩zip中文乱码问题
unzip -O CP936 xxx.zip

### 分区挂载
> #已ext4文件系统格式化分区
mkfs.ext4 /dev/sda1
#将前者(分区)挂载到后者(目录)
mount /dev/sda1 /media/zouy/sda1
#添加卷标
e2label /dev/sda1 software

### 在Ubuntu14.04上安装libgtk2.0-dev
>  如果sudo apt-get install libgtk2.0-dev不行的话用下面的方式

```
#(先n后Y)
sudo aptitude install libgtk2.0-dev 
```