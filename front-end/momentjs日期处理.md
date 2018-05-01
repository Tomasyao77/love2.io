## 简介
> 大家在前端Javascript开发中会遇到处理日期时间的问题，经常会拿来一大堆处理函数才能完成一个简单的日期时间显示效果。moment.js不依赖任何第三方库，支持字符串、Date、时间戳以及数组等格式，可以像PHP的date()函数一样，格式化日期时间，计算相对时间，获取特定时间后的日期时间等等.

## 相关链接
> 中文官网 http://momentjs.cn/

```
git clone https://github.com/moment/moment.git
```
<!--more-->
## 项目常用
**1引入momentjs**
> `<script src="moment.min.js"></script>`


**2注意自定义变量不要取名为moment**
**3常用函数**
```
#当前日期
var now = moment();
console.log(now.format("YYYY-MM-DD HH:mm:ss"));
#或 console.log(moment(new Date()).format("YYYY-MM-DD HH:mm:ss"));
#7天后
console.log(now.add(7,"day").format("YYYY-MM-DD HH:mm:ss"));
#7天前
console.log(now.subtract(7,"day").format("YYYY-MM-DD HH:mm:ss"));
#传字符串并任意格式化
console.log(moment().format("YYYY MM DD"));
console.log(moment().format("YYYY-MM-DD"));
console.log(moment().format("YYYY/MM/DD"));
console.log(moment("2017-9-14").format("YYYY,MM,DD"));
#传时间戳
console.log(moment(1318781876406).format("YYYY-MM-DD HH:mm:ss"));
#传数组 深拷贝 输出2000,2012
var a = moment([2012]);
var b = moment(a);#传date
a.year(2000);
console.log(a.year()+","+b.year());
```
