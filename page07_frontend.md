## JavaScript基本语法

### JavaScript内置对象

​	变量已字母开头 可以用$ _符号开头 大小写敏感

​	使用var 声明变量 一条语句声明多个变量 声明了未赋值就是undefined

​	var a= 10;  var a = 5; 重复定义变量 变量值不会消失

​	

​	数据类型

​		字符串  数字 布尔 数组 对象 Null Undifined

​		都是对象



​	函数

​		无默认值

​		函数内部 var 的变量是局部变量   

​		在函数外声明的变量是全局变量，所有脚本和函数都可以访问全局变量

​	运算符

​		+可以做字符串的拼接

​	流程控制

​		else if 必须分开写



​	数组

​		var arr= new Array();

​		var arr= new Array(size);

​		var arr= new Array(e1,e2…en);

​	

​	Date

​		var date = new Date();



​	Math

​		var pi_value = Math.PI;



​	RegExp

### JavaScript html dom对象

​	window对象

​		window navigation  screen  history location

​	

​	DOM对象

​	document element attr event







### jquery 基础知识

​	jquery选择器

​	jquery事件

​	jquery效果

​	jquery DOM操作

​		属性、值、节点、css，尺寸



## ajax的基本工作原理

​	异步的js和xml

​	通过在后台跟服务器进行少量数据交换，ajax可以使网页实现异步更新

​	XMLHttpRequest 是ajax的基础

​	常用方法  load ajax get  post



 浏览器解析html代码  



css的div的概念和应用



javascript的使用



ajax的问题



css的盒子模型

php的session和server，cookie

http协议过程



html5新特性



JS表单弹出对话框函数是?获得输入焦点函数是?

答:弹出对话框: alert(),prompt(),confirm() 获得输入焦点 focus()



JS的转向函数是?怎么引入一个外部JS文件?

答: window.location.href <script type="text/javascript" src="js/js_function.js"></script>





\4. 前端,HTML,JS

css盒模型。

javascript中的prototype。

javascript中this对象的作用域。





IE和firefox事件冒泡的不同。

什么是怪异模式,标准模式，近标准模式。

DTD的定义

IE/firefox常用hack.

firefox,IE下的前端js/css调试工具。





103、谈谈对javascript的闭包的认识？？？？？

  当函数a的内部函数b被函数a外的一个变量引用的时候，就创建了一个我们通常所谓的“闭包”。



​      1、当内层函数引用了包围它的外层函数体内的变量时，就形成了闭包的一种形式。

​      2、闭包可以避免大量使用全局变量的现象

​      3、内层函数引用的是外层函数中变量的最终值，而不是初始值或中间变换的值。

<http://m.oschina.net/blog/156782>

  



67、你用过多少种JS框架？举例说明优缺点

​    答：jQuery、prototype、dojo、ext、YUI;

​     jQuery:强大的DOM节点查询无人能出其左右，动画操方便； DOM封装的很好！高低版本兼容非常好

​     prototype:较早的jS库，对ajax支持较好，基于原型链面向对象很强大

​     dojo:更容易俣WEB页面具有动态能力；

​     ext:强大的UI操作高居榜首；

​     YUI:强大的类库，提供很多方法；

