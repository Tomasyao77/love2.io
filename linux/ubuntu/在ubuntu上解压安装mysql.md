## 解压安装
**1.从mysql官网下载 https://dev.mysql.com/downloads/mysql/**

> 选择linux-generic linux-generic(glibc 2.5)(x86,64-bit)版本的mysql-5.7.18-linux-glibc2.5-x86_64.tar.gz文件
下载到/data/zouy/install目录下

**2.解压并重命名**
```
cd /data/zouy/install
tar -zxvf mysql-5.7.18-linux-glibc2.5-x86_64.tar.gz
mv mysql-5.7.18-linux-glibc2.5-x86_64 mysql
 ```

<!--more-->

**3.建立data目录(存放数据库等数据)**
```
cd /data/zouy/install/mysql
mkdir mysql-files
```

**4.新建用户(按照官网教程)**
```
sudo groupadd mysql
sudo useradd -r -g mysql -s /bin/false mysql
```

**5.安装mysql(初始化)**
需指定basedir和datadir
```
cd /data/zouy/install/mysql
bin/mysqld --initialize --user=mysql --basedir=/data/zouy/install/mysql --datadir=/data/zouy/install/mysql/mysql-files
#bin/mysql_ssl_rsa_setup --datadir=/data/zouy/install/mysql/mysql-files
```

**6.my.cnf**
> 解压安装自己要配的东西太多了，样例配置文件都没有，还要自己新建。与mysql版本有关。

```
sudo subl /etc/my.cnf
```

`上面的代码是新建一个文件，在mysql.server里有说明要自己新建这个my.cnf配置文件`

我往这个文件里写入的内容如下，是按照mysql.server里说的来做的

> [mysqld]
basedir=/data/zouy/install/mysql
datadir=/data/zouy/install/mysql/mysql-files
skip-grant-tables

`最后一句是启动mysql服务时跳过权限验证`

## 账户配置
**1.配置root用户**
`更改密码过期时间`
```
mysql>use mysql;
mysql>update user set password_expired = 'N' where User = 'root';
```

上一步在启动mysql时跳过了权限验证，这样重启后，我们在shell终端输入mysql直接进入mysql服务器。
```
$ mysql
mysql>UPDATE mysql.user SET authentication_string=PASSWORD('772052352') WHERE User='root';
```

`以上是将root用户的密码修改为772052352,其中mysql比较新的版本密码字段变为了authentication_string`

然后将之前在/etc/my.cnf文件中的*skip-grant-tables*这一句注释掉,如下

> #skip-grant-tables

**2.重启mysql服务**
```
systemctl restart mysql
```

**3.验证前述操作**
```
$ mysql -u root -p
Enter password: 
mysql>
```
`输入密码772052352后就成功进入了mysql服务器`

**4.配置远程访问**
默认root用户只允许本地连接数据库，我们可以通过以下几种方式配置root用户的远程访问权限(*mysql版本不同可能操作会有区别*)

* 改表法

```
$ mysql -u root -p
mysql>use mysql;
mysql>update user set Host = '%' where User = 'root';
```

* 授权法

```
$ mysql -u root -p
mysql>GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' IDENTIFIED BY '772052352' WITH GRANT OPTION;
```

> 这里新建了一个名为admin的用户，密码为772052352，%表示该用户从任何主机都能连接到本mysql服务器。一般我们在这里新建的是名为root的用户，但我观察mysql.user这张表，发现User为root的就会有多个，很奇怪为什么mysql会允许这样操作。

`我推荐用授权法配置远程访问`
