<!-- <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
<link href="https://cdn.bootcss.com/font-awesome/3.2.1/css/font-awesome.min.css" rel="stylesheet">
<script src="https://cdn.bootcss.com/jquery/2.2.4/jquery.min.js"></script>
<script src="https://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script> -->
<script src="https://cdn.bootcss.com/angular.js/1.5.0/angular.min.js"></script>

> 前端基础知识 *`邹曜 D9实验室`*

# 目录
<hr>

* 基本语言（html + css + JavaScript + jquery（js库））
* 框架（css框架bootstrap js框架angularjs）
* js设计模式
* 编码规范
* 开源社区

# 基本语言
<hr>
*结构(html)+表现(css)+行为(javascript)*

> html css

**标签:输入框，标题，段落，图片，列表，链接，表格**

<!--more-->

```bash
<input type="text" name="" value="name" placeholder="请输入name"/>
```
<input type="text" name="" value="name" placeholder="请输入name"/>
```bash
<h1>h1标题</h1>
<h2>h2标题</h2>
<h3>h3标题</h3>
<h4>h4标题</h4>
<h5>h5标题</h5>
<h6>h6标题</h6>
```
![](http://ouq6u283u.bkt.clouddn.com/%E9%80%89%E5%8C%BA_049.png)
```bash
<p>我是第一段落,网址www.divcss5.com ；</p>
<p>我是第二段落；</p>
<p>我是第三段落，<br />我被br换行。</p>
```
<p>我是第一段落,网址www.divcss5.com ；</p>
<p>我是第二段落；</p>
<p>我是第三段落，<br />我被br换行。</p>
```bash
<img src="http://ouq6u283u.bkt.clouddn.com/17-10-24-18:47springfirst.jpg" style="max-height: 200px;"/>
```
<img src="http://ouq6u283u.bkt.clouddn.com/17-10-24-18:47springfirst.jpg" style="max-height: 200px;"/>
```bash
<ul>
    <li>苹果</li>
    <li>香蕉</li>
    <li>西瓜</li>
</ul>
```
<ul>
    <li>苹果</li>
    <li>香蕉</li>
    <li>西瓜</li>
</ul>
```bash
<ol>
    <li>苹果</li>
    <li>香蕉</li>
    <li>西瓜</li>
</ol>
```
<ol>
    <li>苹果</li>
    <li>香蕉</li>
    <li>西瓜</li>
</ol>
```bash
<a href="http://tomasyao.gitee.io/navigation/">点击跳转</a>
```
<a href="http://tomasyao.gitee.io/navigation/">点击跳转</a>
```bash
<table class="table table-bordered table-stripped table-hover">
  <thead>
      <th>姓名</th><th>部门</th><th>月份</th><th>工资</th><th>时间</th>
  </thead>
  <tbody>
      <tr><td>张三</td><td>人事部</td><td>January</td><td>$100</td><td>2017-12-11</td></tr>
      <tr><td>李四</td><td>宣传部</td><td>January</td><td>$200</td><td>2017-12-11</td></tr>
      <tr><td>王五</td><td>营销部</td><td>January</td><td>$300</td><td>2017-12-11</td></tr>
  </tbody>
</table>
```
<div style="max-height: 200px;text-align: center;">
![](http://ouq6u283u.bkt.clouddn.com/%E9%80%89%E5%8C%BA_056.png)
</div>

**div+css盒子模型**

CSS css盒子模型 又称框模型 (Box Model) ，包含了元素内容（content）、内边距（padding）、边框（border）、外边距（margin）几个要素。

<div style="text-align: center;">
![no](http://ouq6u283u.bkt.clouddn.com/%E9%80%89%E5%8C%BA_057.png)
</div>

**浮动,style属性,class属性,css**
```bash
<style type="text/css">
    body {
        background-color: red
    }
    p {
        margin-left: 20px
    }
</style>
```
```bash
<div style="width: 300px;height: 200px;background-color: blue;">
        <div style="width: 100px;height: 100px;background-color: green;"></div>
        <div style="width: 110px;height: 110px;background-color: red;"></div>
</div>
<hr>
<div style="width: 300px;height: 200px;background-color: blue;">
    <div style="width: 100px;height: 100px;background-color: green;float: left;"></div>
    <div style="width: 110px;height: 110px;background-color: red;"></div>
</div>
<hr>
<div style="width: 300px;height: 200px;background-color: blue;">
    <div style="width: 100px;height: 100px;background-color: green;float: left;"></div>
    <div style="width: 110px;height: 110px;background-color: red;float: left;"></div>
</div>
<hr>
<div style="width: 300px;height: 200px;background-color: blue;">
    <div style="width: 100px;height: 100px;background-color: green;float: left;"></div>
    <div style="width: 110px;height: 110px;background-color: red;float: left;margin-left: 10px;margin-top: 10px;"></div>
</div>
```
[演示地址](http://tomasyao.gitee.io/webstorm_ws/d9labNewAngularJs/yanshi.html)
<hr>

**css选择器**

`id选择器`,`类选择器`,`伪类`

```bash
<style>
    .divCss{
        width: 300px;
        height: 200px;
        border: 1px solid red;
        padding: 20px;
    }
    #divId{
        width: 200px;
        height: 100px;
        border: 1px solid greenyellow;
    }
</style>
<div class="divCss">
    <div id="divId">
    </div>
</div>
```
[演示地址](http://tomasyao.gitee.io/webstorm_ws/d9labNewAngularJs/yanshi.html)
<hr>

> JavaScript

JavaScript一种直译式脚本语言，是一种动态类型、弱类型、基于原型的语言，内置支持类型。它的解释器被称为JavaScript引擎，为浏览器的一部分，广泛用于客户端的脚本语言，最早是在HTML（标准通用标记语言下的一个应用）网页上使用，用来给HTML网页增加动态功能。

`控制交互 (人机交互，前后端交互)`，`操作dom`，`样式`，`ajax`，`链式调用`

* 定义变量,对象,函数
```bash
var num = 1;
var name = "zy";
var user = {
    id: 1093,
    username: "zy",
    password: "123456",
    getId: function(){
        return "user的id是: "+this.id;
    }
};
var foo = function(callback){
    var i = 1;
    for(i;i<5;i++){
        //...
    }
    return callback(i);
};
var put = function(p){
    console.log("i的值是: "+p);
};
foo(put);
console.log(user.getId());
```

<script type="text/javascript">
    var num = 1;
var name = "zy";
var user = {
    id: 1093,
    username: "zy",
    password: "123456",
    getId: function(){
        return "user的id是: "+this.id;
    }
};
var foo = function(callback){
    var i = 1;
    for(i;i<5;i++){
        //...
    }
    return callback(i);
};
var put = function(p){
    console.log("i的值是: "+p);
};
foo(put);
console.log(user.getId());
</script>

* 作用域 没有块级作用域,只有函数级作用域
```bash
var faa = function(){
    var a = 1;
    for(;a<10;a++){
        //..
    }  
    console.log(a);
};
faa();
```

<script type="text/javascript">
var faa = function(){
    for(var a = 1;a<10;a++){
        //..
    }  
    console.log(a);
};
faa();
</script>

* 操作dom,样式
```bash
var div = document.getElementById("divId");
console.log(div);
div.onclick = function () {
  if(div.style.backgroundColor === "red"){
      div.style.backgroundColor = "green";
  }else {
      div.style.backgroundColor = "red";
  }
};
```
* ajax异步通信
```bash
var Ajax={
    get: function(url, fn) {
        var obj = new XMLHttpRequest();  // XMLHttpRequest对象用于在后台与服务器交换数据
        obj.open('GET', url, true);
        obj.onreadystatechange = function() {
            if (obj.readyState == 4 && obj.status == 200 || obj.status == 304) { // readyState == 4说明请求已完成
                fn.call(this, obj.responseText);  //从服务器获得数据
            }
        };
        obj.send();
    },
    post: function (url, data, fn) {         // datat应为'a=a1&b=b1'这种字符串格式，在jq里如果data为对象会自动将对象转成这种字符串格式
        var obj = new XMLHttpRequest();
        obj.open("POST", url, true);
        obj.setRequestHeader("Content-type", "application/x-www-form-urlencoded");  // 添加http头，发送信息至服务器时内容编码类型
        obj.onreadystatechange = function() {
            if (obj.readyState == 4 && (obj.status == 200 || obj.status == 304)) {  // 304未修改
                fn.call(this, obj.responseText);
            }
        };
        obj.send(data);
    }
}
```
* 链式调用 `谁是调用者,this就指向谁`
```bash
var f = {
    v: 1,
    m1: function(){
        this.v += 1;
        return this;
    },
    m2: function(){
        this.v += 2;
        return this;
    },
    m3: function(){
        this.v += 3;
        return this;
    }
}
console.log(f.m1().m2().m3());
```

<script type="text/javascript">
    var f = {
    v: 1,
    m1: function(){
        this.v += 1;
        return this;
    },
    m2: function(){
        this.v += 2;
        return this;
    },
    m3: function(){
        this.v += 3;
        return this;
    }
}
console.log(f.m1().m2().m3());
</script>

> jquery

jQuery是一个快速、简洁的JavaScript框架，是继Prototype之后又一个优秀的JavaScript代码库（或JavaScript框架）。jQuery设计的宗旨是“write Less，Do More”，即倡导写更少的代码，做更多的事情。它封装JavaScript常用的功能代码，提供一种简便的JavaScript设计模式，优化HTML文档操作、事件处理、动画设计和Ajax交互。

**Jquery选择器优化，样式,循环，$.ajax()**

```bash
##对比
documen.getElementById("divId");
$("#divId");
documen.getElementByClassName("divCss");
$(".divCss");
$("#inputId").attr("title","123");
$("#divCss").css("color","red");
var list = [1,2,3,4,5];
for(var i=0,list;i<list.length;i++){
    //...
}
$.each(list,function(k,v){
    //.
});
$.ajax({
    url: ,
    type: '',
    dataType: '',
    data: {
          
    },
    success: function(){
         
    },
    error: function(){
          
    }
 })
```

# 框架
<hr>
**可复用组件，减少开发时间，提高开发效率，节省成本，提升产品质量。**

> Bootstrap 

Bootstrap：移动设备优先，浏览器支持，容易上手，响应式设计。

`网格布局（12）`，`表单（输入标签，按钮）`，`表格`，`面板`，`导航`，`分页`，`进度条`，`模态框`

* 网格布局 
*Bootstrap 包含了一个响应式的、移动设备优先的、不固定的网格系统，可以随着设备或视口大小的增加而适当地扩展到 12 列*
![](http://ouq6u283u.bkt.clouddn.com/%E9%80%89%E5%8C%BA_050.png)
* 表单元素
```bash
<form class="form-horizontal" role="form">
        <div class="form-group">
            <label class="col-sm-2 control-label">用户名</label>
            <div class="col-sm-10">
                <input type="text" class="form-control" placeholder="请输入用户名"/>
            </div>
        </div>
        <div class="form-group">
            <label class="col-sm-2 control-label">密码</label>
            <div class="col-sm-10">
                <input type="password" class="form-control" placeholder="请输入密码"/>
            </div>
        </div>
        <div class="form-group">
            <label class="col-sm-2 control-label">性别</label>
            <div class="col-sm-10">
                <label class="radio-inline">
                    <input type="radio" name="sexRadio" value="man" checked> 男
                </label>
                <label class="radio-inline">
                    <input type="radio" name="sexRadio" value="woman"> 女
                </label>
            </div>
        </div>
        <div class="form-group">
            <label class="col-sm-2 control-label">爱好</label>
            <div class="col-sm-10">
                <select class="form-control">
                    <option>篮球</option>
                    <option>足球</option>
                    <option>游戏</option>
                    <option>跑步</option>
                    <option>学习</option>
                </select>
            </div>
        </div>
        <div class="form-group">
            <div class="col-sm-offset-2 col-sm-10">
                <div class="checkbox">
                    <label>
                        <input type="checkbox">请记住我
                    </label>
                </div>
            </div>
        </div>
        <div class="form-group">
            <div class="col-sm-offset-2 col-sm-10">
                <button type="button" class="btn btn-default">登录</button>
                <button type="button" class="btn btn-primary">登录</button>
                <button type="button" class="btn btn-info">登录</button>
                <button type="button" class="btn btn-danger">登录</button>
                <button type="button" class="btn btn-warning">登录</button>
            </div>
        </div>
    </form>
```
[演示地址](http://tomasyao.gitee.io/webstorm_ws/d9labNewAngularJs/yanshi.html)
<hr>
* 表格 面板 分页
![](http://ouq6u283u.bkt.clouddn.com/%E9%80%89%E5%8C%BA_051.png)

* 模态框
```bash
<div class="modal fade" data-backdrop="static" id="{{modalId}}" tabindex="-1" role="dialog">
    <!--单击背景时不关闭: data-backdrop="static"-->
    <div class="modal-dialog modal-lg" role="document">
        <div class="modal-content">
            <div class="modal-header" style="position: relative;">
                <button type="button" class="close" data-dismiss="modal"><span>&times;</span></button>
                <h4 class="modal-title"><span ng-bind='title'></span></h4>
                <div class="text-center" style="position: absolute;top: 15px;left: 0;width: 100%;">
                    <span ng-show="entity.modalTopAlertShow" class="alert alert-danger"
                          style="padding: 6px 10px;">请将信息填写完整</span>
                </div>
            </div>
            <div class="modal-body form-horizontal" ng-click="modalBodyClick()">
                <div ng-transclude="body"></div>
            </div>
            <div class="modal-footer">
                <span ng-if="action==='add' || action==='edit'">
                    <button type="button" class="btn btn-default" data-dismiss="modal">关闭</button>
                    <button type="button" class="btn btn-success" ng-click='submit()'>
                        <span class="icon-ok m-r"></span>&nbsp;提交
                    </button>
                </span>
                <span ng-if="action==='view'">
                    <button type="button" class="btn btn-default" data-dismiss="modal">关闭</button>
                </span>
                <span ng-transclude="footer"></span>
            </div>
        </div>
    </div>
</div>
```
    ![](http://ouq6u283u.bkt.clouddn.com/%E9%80%89%E5%8C%BA_053.png)

> Angularjs

AngularJS[1]  诞生于2009年，由Misko Hevery 等人创建，后为Google所收购。是一款优秀的前端JS框架，已经被用于Google的多款产品当中。AngularJS有着诸多特性，最为核心的是：MVC、模块化、自动化双向数据绑定、语义化标签、依赖注入等等。

MVW (Model-View-Whatever)模式

指令:ng-app，ng-controller，ng-factory，ng-service，ng-provider，ng-init，ng-model，ng-bind，ng-show，ng-hide，ng-if，ng-repeat，ng-options，ng-bind-html

`创建自定义的指令`： `.directive` 函数。使用驼峰法来命名一个指令， myDirective, 但在使用它时需要以 - 分割, my-directive。

```bash
<script>
    angular.module("m",[])
        .controller("c",function ($scope,myFactory) {
            $scope.entity = myFactory.getUser();
            console.log($scope.entity);
            console.log($scope.entity.getValue("username"));
        })
        .factory("myFactory",function () {//.service .provider
            return {
                getUser: function () {
                    var user = {
                        username: "zouy",
                        password: "123456",
                        age: 22,
                        getValue: function (v) {
                            return this[v];
                        }
                    };
                    return user;
                }
            }
        })
        .directive("myDirective",function () {
            return {
                restrict: 'AEC',//A:attribute E:element C:class
                replace: true,
                scope: {
                    entity: "=", title: "@", name: "@"
                },
                template: '<input class="" style="" type="text" ng-model="entity[name]"' +
                ' ng-click="click()" ng-focus="focus()" ng-blur="blur()" />',
                link: function (scope) {
                    scope.click = function () {
                        //...
                    };
                    scope.focus = function () {
                        //...
                    };
                    scope.blur = function () {
                        //...
                    };
                    //......
                }
            }
        })
</script>
```
```bash
<div ng-app="m" ng-controller="c" style="margin-bottom: 20px;text-align: center;">
    <div my-directive name="username" title="用户名" entity="entity"></div>
    <div my-directive name="password" title="密码" entity="entity"></div>
    <div my-directive name="age" title="年龄" entity="entity"></div>
    双向数据绑定:
    <input type="text" ng-model="entity.username">
    <input type="text" ng-model="entity.password">
    <input type="text" ng-model="entity.age">
</div>
```

<script>
    angular.module("m",[])
        .controller("c",function ($scope,myFactory) {
            $scope.entity = myFactory.getUser();
            console.log($scope.entity);
            console.log($scope.entity.getValue("username"));
        })
        .factory("myFactory",function () {//.service .provider
            return {
                getUser: function () {
                    var user = {
                        username: "zouy",
                        password: "123456",
                        age: 22,
                        getValue: function (v) {
                            return this[v];
                        }
                    };
                    return user;
                }
            }
        })
        .directive("myDirective",function () {
            return {
                restrict: 'AEC',//A:attribute E:element C:class
                replace: true,
                scope: {
                    entity: "=", title: "@", name: "@"
                },
                template: '<input class="" style="" type="text" ng-model="entity[name]"' +
                ' ng-click="click()" ng-focus="focus()" ng-blur="blur()" />',
                link: function (scope) {
                    scope.click = function () {
                        //...
                    };
                    scope.focus = function () {
                        //...
                    };
                    scope.blur = function () {
                        //...
                    };
                    //......
                }
            }
        })
</script>

<div ng-app="m" ng-controller="c" style="margin-bottom: 20px;text-align: center;">
    <div my-directive name="username" title="用户名" entity="entity"></div>
    <div my-directive name="password" title="密码" entity="entity"></div>
    <div my-directive name="age" title="年龄" entity="entity"></div>
    双向数据绑定:
    <input type="text" ng-model="entity.username">
    <input type="text" ng-model="entity.password">
    <input type="text" ng-model="entity.age">
</div>

应用: 表单,模态框等可重用组件

# js设计模式
<hr>
**设计模式**是对软件设计中一些反复出现，普遍存在的问题所提出的解决方案。

`单体模式`：单体是一个用来划分命名空间并将一批相关的属性和方法组织在一起的对象，如果他可以被实例化，那么他只能被实例化一次。
```bash
var Singleton = {
    attribute: true,
    method1: function(){},
    method2: function(){}
};
```
`单例模式`：就是保证一个类只有一个实例。
```bash
var single = (function(){
    var unique;

    function getInstance(){
　　　　// 如果该实例存在，则直接返回，否则就对其实例化
        if( unique === undefined ){
            unique = new Construct();
        }
        return unique;
    }

    function Construct(){
        // ... 生成单例的构造函数的代码
    }

    return {
        getInstance : getInstance
    }
})();
```
`工厂模式`：提供创建对象的接口，意思就是根据领导（调用者）的指示（参数），生产相应的产品（对象）。
分为简单工厂模式和复杂工厂模式
```bash
简单工厂模式：　var XMLHttpFactory =function(){};　　　　　　//这是一个简单工厂模式
　　XMLHttpFactory.createXMLHttp =function(){
　　　 var XMLHttp = null;
　　　　if (window.XMLHttpRequest){
　　　　　　XMLHttp = new XMLHttpRequest()
　　　 }else if (window.ActiveXObject){
　　　　　　XMLHttp = new ActiveXObject("Microsoft.XMLHTTP")
　　　　}
　　return XMLHttp;
　　}
　　//XMLHttpFactory.createXMLHttp()这个方法根据当前环境的具体情况返回一个XHR对象。
　　var AjaxHander =function(){
　　　　var XMLHttp = XMLHttpFactory.createXMLHttp();
　　　　...
}
```

# 编码规范
<hr>
> 通用

* **命名**（变量，函数，文件，目录）
* `驼峰式`,`模块化`,`可读性`
**ajaxModule**
**pageModule**
**entityModalView**
**userManage.jsp**
**util.js**
**main.css** 
**login.css**
![](http://ouq6u283u.bkt.clouddn.com/%E9%80%89%E5%8C%BA_054.png)

> html

* 单标签 双标签 标签闭合
![](http://ouq6u283u.bkt.clouddn.com/%E9%80%89%E5%8C%BA_055.png)
```bash
<div></div>
<span></span>
<h3></h3>
<input type="" name=""/>
<img src="" alt=""/>
```
* style里每个属性用；做结尾，比如style=“position：absolute;top: 10px;left: 0;”

> css

* 颜色十六进制，尽可能短例如使用 #fff 替代#ffffff，且尽量使用小写字母，以使在浏览文档时，它们能够更轻松的被区分开来。
* 不要为 0 指明单位，比如使用 margin: 0; 而不是 margin: 0px;。

> JavaScript

* 不要声明全局变量，比如a = 1;应改为var a = 1;
* 声明之后一律以分号结束， 不可以省略;比var foo = function(){};var b = “str”;
* 尽量避免避免 ==和!= 的使用， 用严格比较条件 ===和!==

# 开源社区
<hr>
* Github
    [我的github](https://github.com/Tomasyao77/)
* 码云
    [我的gitee](https://gitee.com/tomasyao/)
* 前端好的学习网站:
[慕课网](https://www.imooc.com/)
[菜鸟教程](http://www.runoob.com/)
* 技术工具
[在线代码格式化](http://tool.oschina.net/codeformat)
[js日期处理](http://momentjs.cn/)
[js图表](http://www.bootcss.com/p/chart.js/)

> git Pull Request工作流程

* fork项目,到网站上点击一下fork按钮，将项目复制到自己的工程中。
* 修改bug
* 发送一个Pull Request
* 原作者觉得可以就merge
* 一次协作完成

![](http://ouq6u283u.bkt.clouddn.com/thanks.jpg)