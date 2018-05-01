## 安装前准备
**1 操作环境**
> 服务器：阿里云 1核cpu 2G内存 1M带宽 40G云盘
操作系统：centos7.3

<!--more-->

**2 在系统根目录建立/data/zouy/install目录结构(个人习惯)**
```
cd /
mkdir /data/zouy/install
chmod -R 755 /data #修改权限
```
**3 用各种ftp客户端将本地安装包上传到服务器**
这里我直接在自己本地的Ubuntu系统上用scp命令上传(需输入密码)
```
scp /home/zouy/jdk-8u144-linux-x64.tar.gz root@30.108.180.232:/data/zouy/install
scp /home/zouy/apache-tomcat-8.5.20.tar.gz root@30.108.180.232:/data/zouy/install
```

`上述命令中，/home/zouy为我本地目录，30.108.180.232为远程服务器地址。且除了步骤3，其他步骤中的命令均在服务器中书写。`

## 安装配置
**1 安装配置jdk**
1) 解压
```
cd /data/zouy/install
tar -zxvf jdk-8u144-linux-x64.tar.gz
mv jdk-8u144-linux-x64 jdk1.8.0_144 #改名
```
2) 配置环境变量
```
vi /etc/profile
```
在/etc/profile文件中加入下述内容
> export JAVA_HOME=/data/zouy/install/jdk1.8.0_144
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

**2 安装配置tomcat**
1) 解压
```
cd /data/zouy/install
tar -zxvf apache-tomcat-8.5.20.tar.gz
mv apache-tomcat-8.5.20 tomcat #改名
```
2) 修改catalina.sh(配置java环境以及tomcat.pid)
```
cd /data/zouy/install/tomcat/bin
vi catalina.sh
```
在文件开头加入下面两行
> export JAVA_HOME=/data/zouy/install/jdk1.8.0_144
export JRE_HOME=/data/zouy/install/jdk1.8.0_144/jre

在文件第139行左右的位置大概有如下内容
> #Copy CATALINA_BASE from CATALINA_HOME if not already set
[ -z "$CATALINA_BASE" ] && CATALINA_BASE="$CATALINA_HOME"

在下面加上一行
> #Copy CATALINA_BASE from CATALINA_HOME if not already set
[ -z "$CATALINA_BASE" ] && CATALINA_BASE="$CATALINA_HOME"
CATALINA_PID="$CATALINA_BASE/tomcat.pid"

3) 安装rng-tools
`centos7下tomcat启动很慢，解决的方法是安装熵服务`
```
yum install rng-tools  
systemctl start rngd  
```
4) 配置tomcat服务
```
#在/usr/lib/systemd/system下面建立tomcat.service文件
cd /usr/lib/systemd/system
vi tomcat.service
```
在tomcat.service文件中写人以下内容
> [Unit]
Description=Tomcat
After=syslog.target network.target remote-fs.target nss-lookup.target
[Service]
Type=forking
Enviroment="JAVA_HOME=/data/zouy/install/jdk1.8.0_144"
PIDFile=/data/zouy/install/tomcat/tomcat.pid
ExecStart=/data/zouy/install/tomcat/bin/startup.sh
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true
[Install]
WantedBy=multi-user.target

5) 服务相关命令
> systemctl enable tomcat  开机启动
systemctl status tomcat   查看状态
systemctl start / stop / restart tomcat    开始/停止/重启 tomcat

6) 防火墙开放端口
`这里先要说明下，即使开放了比如8080端口，启动tomcat后也是不能直接访问的。因为阿里云设置了入站规则，需要到阿里云管理控制台设置，进入实例->本实例安全组->配置规则->添加安全组规则 进行配置即可。`
```
yum install firewalld #安装
systemctl start firewalld #启动
firewall-cmd --add-port=8080/tcp #添加8080端口
firewall-cmd --reload #更新规则
firewall-cmd --state #查看状态 
```
**3 安装配置nginx**
1) 解压
`我下载的压缩包放到/usr/local目录下`
```
cd /usr/local
tar -zxvf nginx-1.12.1.tar.gz
```
2) 在安装nginx前先安装gcc-c++ pcre zlib openssl等依赖
```
yum install -y gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel 
```
3) 编译安装
```
cd /usr/local/nginx-1.12.1
./configure --with-prefix=/usr/local/nginx --with-http_ssl_module  
#注意上一步要仔细看输出结果，如果pcre,zlib ,openssl显示found才继续下一步，否则仔细找原因为什么依赖库没装上
make  
make install  
```
4) 配置环境变量
```
vi ~/.bashrc
```
加入一句话
> export PATH=$PATH:/usr/local/nginx/sbin

5) 运行/停止 nginx
```
#运行
nginx
#停止
nginx -s stop
#重启
nginx -s reload
```








