---
title: angularjs derective之ng-transclude
date: 2017-10-08 14:57:24
tags: [angularjs,derective]
---
## 特别感谢
https://segmentfault.com/a/1190000004586636
深受此篇文章影响,受益匪浅,非常感谢.
### 1 基础页面框架
引入bootstarp jquery angular相关文件
![](http://ouq6u283u.bkt.clouddn.com/hexoblog/img17-10-8-15:50-firststep.png)
<!--more-->
### 2 html body相关结构
必要的指令元素于普通元素
![](http://ouq6u283u.bkt.clouddn.com/hexoblog/img17-10-8-16:03-body.png)
### 3 直接template指令
![](http://ouq6u283u.bkt.clouddn.com/hexoblog/img17-10-8-16:01-directive.png)
**结果**
![](http://ouq6u283u.bkt.clouddn.com/hexoblog/img17-10-8-16:14-result1.png)
### 4 使用templateUrl
![](http://ouq6u283u.bkt.clouddn.com/hexoblog/img17-10-8-16:06-templateurl.png)
其中text_template.html文件内容为
![](http://ouq6u283u.bkt.clouddn.com/hexoblog/img17-10-8-16:09-temhtml.png)
**结果**
![](http://ouq6u283u.bkt.clouddn.com/hexoblog/img17-10-8-16:14-result2.png)
### 5 总结
这篇文章没写好,以后删了重写