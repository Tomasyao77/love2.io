## 安装fastdfs相关
**1.libfastcommon fastdfs通用函数库**

```bash
git clone https://github.com/happyfish100/libfastcommon.git /home/zouy/download
```

**2.fastdfs**

```bash
git clone https://github.com/happyfish100/fastdfs.git /home/zouy/download
```

<!--more-->

`其中，/home/zouy/download改成自己的目录，用于存放下载的文件`

**3.编译安装libfastcommon和fastdfs**

```bash
cd /home/zouy/download/libfastcommon
./make.sh
sudo ./make.sh install
cd /home/zouy/download/fastdfs
./make.sh
sudo ./make.sh install
```

**4.建立日志目录 和 用于存放文件的目录**

```bash
cd /home/zouy
mkdir log
mkdir fileSave
```

`/home/zouy改成自己的目录`

**6.修改配置文件**

```bash
cd /etc/fdfs
```

分别修改*tracker.conf.sample* *storage.conf.sample* *client.conf.sample*三个文件

主要就是改日志文件目录，文件存储目录以及tracker服务器地址

* vi tracker.conf.sample

> base_path=/home/zouy/log
http.server_port=80

* vi storage.conf.sample

> base_path=/home/zouy/log
store_path0=/home/zouy/fileSave
tracker_server=yourServerIp:22122
http.server_port=80

`yourServerIp改成自己服务器的ip地址`

* vi client.conf.sample

> base_path=/home/zouy/log
http.tracker_server_port=80

**这里先不启动fastdfs服务，先把nginx配好**

## 安装nginx相关
**1.下载nginx 和 fastdfs-nginx-module安装包**

```bash
cd /home/zouy/download
wget https://github.com/happyfish100/fastdfs-nginx-module/archive/master.zip
unzip master
wget http://nginx.org/download/nginx-1.9.5.tar.gz
tar -zxvf nginx-1.9.5.tar.gz
```

`其中，/home/zouy/download改成自己的目录，用于存放下载的文件`
`我下载下来的fastdfs-nginx-module文件名是master`

**2.安装nginx 和 fastdfs-nginx-module模块**

```bash
mv nginx-1.9.5 /usr/local/
cd /usr/local/nginx-1.9.5
./configure --add-module=/home/zouy/download/fastdfs-nginx-module-master/src/  --prefix=/usr/local/nginx --user=nobody --group=nobody  --with-http_gzip_static_module --with-http_gunzip_module
make
make install
```

`待会配置nginx的时候需要用到fastdfs-nginx-module模块`

**3.修改fastdfs-nginx-module里的mod_fastdfs.conf文件**

* vi /home/zouy/download/fastdfs-nginx-module-master/src/mod_fastdfs.conf

> base_path=/home/zouy/log
tracker_server=yourServerIp:22122
store_path0=/home/zouy/fileSave
url_have_group_name = true

`/home/zouy/改成自己的目录，yourServerIp改成自己服务器的ip地址`

**4.将mod_fastdfs.conf http.conf mime.types复制到/etc/fdfs下**

```bash
cp /home/zouy/download/fastdfs-nginx-module-master/src/mod_fastdfs.conf /etc/fdfs/
cp /home/zouy/download/fastdfs/http.conf /etc/fdfs/
cp /home/zouy/download/fastdfs/mime.types /etc/fdfs/
```
**5.修改nginx配置文件**

* vi /usr/local/nginx/conf/nginx.conf

> location /group1/M00 {
        root /home/zouy/fileSave;
        ngx_fastdfs_module;
}

## 启动fastdfs和nginx

**1.启动fastdfs分发和存储服务**

```bash
/usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf
/usr/bin/fdfs_storaged /etc/fdfs/storage.conf
```

**2.启动nginx**

```bash
/usr/local/nginx/sbin/nginx
```
**3.查看服务是否启动**

```bash
ps -ef | grep fdfs
ps -ef | grep nginx
```

## 测试图床是否搭建成功

* 上传图片

```bash
fdfs_test /etc/fdfs/client.conf upload /home/fe/test.jpg
```
* 在浏览器查看图片图片

```bash
http://119.29.141.240/group1/M00/00/00/Cmh6v1lhj0KAPwzOABFdL646mtk074.jpg
```

* 如下图

<img src="http://119.29.141.240/group1/M00/00/00/Cmh6v1lh9RiASYreABFdL646mtk324.jpg" width = "80%" alt="图片名称" align=center />
