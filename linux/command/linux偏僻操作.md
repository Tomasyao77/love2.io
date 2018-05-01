---
title: linux偏僻操作
date: 2017-08-02 16:41:31
tags: [linux]
---
## 压缩解压
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

<!--more-->
## 分区挂载
> #已ext4文件系统格式化分区
mkfs.ext4 /dev/sda1
#将前者(分区)挂载到后者(目录)
mount /dev/sda1 /media/zouy/sda1
#添加卷标
e2label /dev/sda1 software

## 在Ubuntu14.04上安装libgtk2.0-dev
>  如果sudo apt-get install libgtk2.0-dev不行的话用下面的方式

```
#(先n后Y)
sudo aptitude install libgtk2.0-dev 
```

