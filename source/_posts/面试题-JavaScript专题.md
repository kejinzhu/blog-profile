---
title: 面试题-JavaScript专题
copyright: true
date: 2018-08-30 23:45:52
categories: 面试题
tags:
    - 面试题
    - JavaScript专题
---
## 介绍js的基本数据类型
undefined、null、boolean、number、string
## js有哪些内置对象？

- 数据封装类对象：Object、Array、Boolean、Number 和 String
- 其他对象：Function、Arguments、Math、Date、RegExp、Error

## this对象的理解

- this总是指向函数的直接调用者（而非间接调用者）；
- 如果有new关键字，this指向new出来的那个对象；
- 在事件中，this指向触发这个事件的对象，特殊的是，IE中的attachEvent中的this总是指向全局对象Window；
<!-- more -->
## eval是做什么的？
它的功能是把对应的字符串解析成JS代码并运行；
应该避免使用eval，不安全，非常耗性能（2次，一次解析成js语句，一次执行）。
由JSON字符串转换为JSON对象的时候可以用eval，var obj =eval('('+ str +')');
## DOM怎样添加、移除、移动、复制、创建和查找节点

```javascript
// 创建新节点
createDocumentFragment()    //创建一个DOM片段
createElement()   //创建一个具体的元素
createTextNode()   //创建一个文本节点
// 添加、移除、替换、插入
appendChild()
removeChild()
replaceChild()
insertBefore() //在已有的子节点前插入一个新的子节点
// 查找
getElementsByTagName()    //通过标签名称
getElementsByName()    //通过元素的Name属性的值(IE容错能力较强，会得到一个数组，其中包括id等于name值的)
getElementById()    //通过元素Id，唯一性
```
## null和undefined的区别？
- null 		表示一个对象是“没有值”的值，也就是值为“空”；
- undefined 	表示一个变量声明了没有初始化(赋值)；

- undefined不是一个有效的JSON，而null是；
- undefined的类型(typeof)是undefined；
- null的类型(typeof)是object；

Javascript将未赋值的变量默认值设为undefined；
Javascript从来不会将变量设为null。它是用来让程序员表明某个用var声明的变量时没有值的。

typeof undefined
 	//"undefined"
 	undefined :是一个表示"无"的原始值或者说表示"缺少值"，就是此处应该有一个值，但是还没有定义。当尝试读取时会返回 undefined；
 	例如变量被声明了，但没有赋值时，就等于undefined

typeof null
 	//"object"
 	null : 是一个对象(空对象, 没有任何属性和方法)；
 	例如作为函数的参数，表示该函数的参数不是对象；

注意：
 	在验证null时，一定要使用　=== ，因为 == 无法分别 null 和　undefined
 	null == undefined // true
 	null === undefined // false

null是一个表示"无"的对象，转为数值时为0；undefined是一个表示"无"的原始值，转为数值时为NaN。

### ** undefined：**
- （1）变量被声明了，但没有赋值时，就等于undefined。
- （2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。
- （3）对象没有赋值的属性，该属性的值为undefined。
- （4）函数没有返回值时，默认返回undefined。

### ** null：**
- （1） 作为函数的参数，表示该函数的参数不是对象。
- （2） 作为对象原型链的终点。

具体差别可查看这篇文章：http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html
## new操作符具体干了什么呢?

-（1）创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。
-（2）属性和方法被加入到 this 引用的对象中。
-（3）新创建的对象由 this 所引用，并且最后隐式的返回 this 。
## JSON 的了解？

JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。它是基于JavaScript的一个子集。数据格式简单, 易于读写, 占用带宽小。
格式：采用键值对，例如：{'age':'12', 'name':'back'}
## call() 和 apply() 的区别和作用？

apply()函数有两个参数：第一个参数是上下文，第二个参数是参数组成的数组。如果上下文是null，则使用全局对象代替。
如：function.apply(this,[1,2,3]);
call()的第一个参数是上下文，后续是实例传入的参数序列。
如：function.call(this,1,2,3);
## 如何获取UA（用户代理）？

```javascript
function whatBrowser() {  
    document.Browser.Name.value=navigator.appName;  
    document.Browser.Version.value=navigator.appVersion;          document.Browser.Code.value=navigator.appCodeName;  
    document.Browser.Agent.value=navigator.userAgent;  
}  
navigator.userAgent 获取浏览器UA
```

## 说几条写JavaScript的基本规范？

- 1.不要在同一行声明多个变量。
- 2.请使用 ===/!==来比较true/false或者数值
- 3.使用对象字面量替代new Array这种形式
- 4.不要使用全局函数。
- 5.Switch语句必须带有default分支
- 6.函数不应该有时候有返回值，有时候没有返回值。
- 7.For循环必须使用大括号
- 8.If语句必须使用大括号
- 9.for-in循环中的变量 应该使用var关键字明确限定作用域，从而避免作用域污染。

## JavaScript原型，原型链 ? 有什么特点？

- （1）原型对象也是普通的对象，是对象一个自带隐式的 __proto__ 属性，原型也有可能有自己的原型，如果一个原型对象的原型不为null的话，我们就称之为原型链。
- （2）原型链是由一些用来继承和共享属性的对象组成的（有限的）对象链。

每个对象都会在其内部初始化一个属性，就是prototype(原型)，当我们访问一个对象的属性时，
如果这个对象内部不存在这个属性，那么他就会去prototype里找这个属性，这个prototype又会有自己的prototype，
于是就这样一直找下去，也就是我们平时所说的原型链的概念。
关系：instance.constructor.prototype = instance.__proto__

特点：
JavaScript对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。


当我们需要一个属性的时，Javascript引擎会先看当前对象中是否有这个属性， 如果没有的话，就会查找他的Prototype对象是否有这个属性，如此递推下去，一直检索到 Object 内建对象。

```javascript
function Func(){}
    Func.prototype.name = "Sean";
 	Func.prototype.getInfo = function() {
 	  return this.name;
}
var person = new Func();//现在可以参考var person = Object.create(oldObject);
console.log(person.getInfo());//它拥有了Func的属性和方法
//"Sean"
console.log(Func.prototype);
// Func { name="Sean", getInfo=function()}
```

## JavaScript有几种类型的值？，你能画一下他们的内存图吗？

- 栈：原始数据类型（Undefined，Null，Boolean，Number、String）
- 堆：引用数据类型（对象、数组和函数）

两种类型的区别是：存储位置不同；

- 原始数据类型直接存储在栈(stack)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；
- 引用数据类型存储在堆(heap)中的对象,占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体

## Flash、Ajax各自的优缺点，在使用中如何取舍？

Flash适合处理多媒体、矢量图形、访问机器；对CSS、处理文本上不足，不容易被搜索。
Ajax对CSS、文本支持很好，支持搜索；多媒体、矢量图形、机器访问不足。
共同点：与服务器的无刷新传递消息、用户离线和在线状态、操作DOM
## 什么是闭包？为什么要用它？

### 闭包的理解
闭包，官方对闭包的解释是：一个拥有许多变量和绑定了这些变量的环境的表达式（通常是一个函数），因而这些变量也是该表达式的一部分。

闭包的特点：
- （1）作为一个函数变量的一个引用，当函数返回时，其处于激活状态。
- （2） 一个闭包就是当一个函数返回时，一个没有释放资源的栈区。

闭包的特性：

- 1.函数内再嵌套函数
- 2.内部函数可以引用外层的参数和变量
- 3.参数和变量不会被垃圾回收机制回收

简单的说，Javascript允许使用内部函数---即函数定义和函数表达式位于另一个函数的函数体内。而且，这些内部函数可以访问它们所在的外部函数中声明的所有局部变量、参数和声明的其他内部函数。当其中一个这样的内部函数在包含它们的外部函数之外被调用时，就会形成闭包。

### 为什么要用闭包？

```javascript
//li节点的onclick事件都能正确的弹出当前被点击的li索引
  <ul id="testUL">
     <li> index = 0</li>
     <li> index = 1</li>
     <li> index = 2</li>
     <li> index = 3</li>
 </ul>
 <script type="text/javascript">
   	var nodes = document.getElementsByTagName("li");
 	for(i = 0;i<nodes.length;i+= 1){
 	    nodes[i].onclick = (function(i){
 	              return function() {
 	                 console.log(i);
 	              } //不用闭包的话，值每次都是4
 	            })(i);
 	}
 </script>
```

执行say667()后,say667()闭包内部变量会存在,而闭包内部函数的内部变量不会存在
使得Javascript的垃圾回收机制GC不会收回say667()所占用的资源
因为say667()的内部函数的执行需要依赖say667()中的变量
这是对闭包作用的非常直白的描述

```javascript
function say667() {
 	// Local variable that ends up within closure
 	var num = 666;
 	var sayAlert = function() {
 		alert(num);
 	}
 	num++;
 	return sayAlert;
 }

var sayAlert = say667();
sayAlert()//执行结果应该弹出的667
```

## javascript 代码中的"use strict";是什么意思 ? 使用它区别是什么？

 use strict是一种ECMAscript 5 添加的（严格）运行模式,这种模式使得 Javascript 在更严格的条件下运行,

 使JS编码更加规范化的模式,消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为。
 默认支持的糟糕特性都会被禁用，比如不能用with，也不能在意外的情况下给全局变量赋值;
 全局变量的显示声明,函数必须声明在顶层，不允许在非函数代码块内声明函数,arguments.callee也不允许使用；
 消除代码运行的一些不安全之处，保证代码运行的安全,限制函数中的arguments修改，严格模式下的eval函数的行为和非严格模式的也不相同;

 提高编译器效率，增加运行速度；
 为未来新版本的Javascript标准化做铺垫。

## Javascript中，有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是？

hasOwnProperty

javaScript中hasOwnProperty函数方法是返回一个布尔值，指出一个对象是否具有指定名称的属性。此方法无法检查该对象的原型链中是否具有该属性；该属性必须是对象本身的一个成员。

使用方法：
object.hasOwnProperty(proName)
其中参数object是必选项。一个对象的实例。
proName是必选项。一个属性名称的字符串值。

如果 object 具有指定名称的属性，那么JavaScript中hasOwnProperty函数方法返回 true，反之则返回 false。

## js延迟加载的方式有哪些？

defer和async、动态创建DOM方式（用得最多）、按需异步载入js

## javascript里面的继承怎么实现，如何避免原型链上面的对象共享

用构造函数和原型链的混合模式去实现继承，避免对象共享可以参考经典的extend()函数，很多前端框架都有封装的，就是用一个空函数当做中间变量
## ajax过程

- (1)创建XMLHttpRequest对象,也就是创建一个异步调用对象.
- (2)创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息.
- (3)设置响应HTTP请求状态变化的函数.
- (4)发送HTTP请求.
- (5)获取异步调用返回的数据.
- (6)使用JavaScript和DOM实现局部刷新.

## Ajax 解决浏览器缓存问题？

- 1、在ajax发送请求前加上 anyAjaxObj.setRequestHeader("If-Modified-Since","0")。
- 2、在ajax发送请求前加上 anyAjaxObj.setRequestHeader("Cache-Control","no-cache")。
- 3、在URL后面加上一个随机数： "fresh=" + Math.random();。
- 在URL后面加上时间戳："nowtime=" + new Date().getTime();。
- 如果是使用jQuery，直接这样就可以了 $.ajaxSetup({cache:false})。这样页面的所有ajax都会执行这条语句就是不需要保存缓存记录。

## 同步和异步的区别？
同步的概念应该是来自于OS中关于同步的概念:不同进程为协同完成某项工作而在先后次序上调整(通过阻塞,唤醒等方式).同步强调的是顺序性.谁先谁后.异步则不存在这种顺序性.

### 同步：
浏览器访问服务器请求，用户看得到页面刷新，重新发请求,等请求完，页面刷新，新内容出现，用户看到新内容,进行下一步操作。

### 异步：
浏览器访问服务器请求，用户正常操作，浏览器后端进行请求。等请求完，页面不刷新，新内容也会出现，用户看到新内容。

## AMD和CMD的区别？

- AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。
- CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。

类似的还有 CommonJS Modules/2.0 规范，是 BravoJS 在推广过程中对模块定义的规范化产出。

这些规范的目的都是为了 JavaScript 的模块化开发，特别是在浏览器端的。
目前这些规范的实现都能达成浏览器端模块化开发的目的。

区别：

1. 对于依赖的模块，AMD 是提前执行，CMD 是延迟执行。不过 RequireJS 从 2.0 开始，也改成可以延迟执行（根据写法不同，处理方式不同）。CMD 推崇 as lazy as possible.

2. CMD 推崇依赖就近，AMD 推崇依赖前置。

```javascript
// CMD
define(function(require, exports, module) {
    var a = require('./a') a.doSomething()
    // 此处略去 100 行   
    var b = require('./b')
    // 依赖可以就近书写   
        b.doSomething()
    // ... 
})
// AMD 默认推荐的是
define(['./a', './b'], function(a, b) { // 依赖必须一开始就写好    
        a.doSomething()
// 此处略去 100 行   
        b.doSomething()
})
```
## ajax不可避免的问题都有什么？如何解决呢？
- （Q1）ajax以何种数据格式交换数据和跨域的问题如何解决

- （Q2）这两大问题，都有不同的解决方案，但是最被推崇的就是用JSON来传数据，靠JSONP来跨域
## 你有哪些性能优化的方法？
- （1） 减少http请求次数：CSS Sprites, JS、CSS源码压缩、图片大小控制合适；网页Gzip，CDN托管，data缓存 ，图片服务器。
- （2） 前端模板 JS+数据，减少由于HTML标签导致的带宽浪费，前端用变量保存AJAX请求结果，每次操作本地变量，不用请求，减少请求次数
- （3） 用innerHTML代替DOM操作，减少DOM操作次数，优化javascript性能。
- （4） 当需要设置的样式很多时设置className而不是直接操作style。
- （5） 少用全局变量、缓存DOM节点查找的结果。减少IO读取操作。
- （6） 避免使用CSS Expression（css表达式)又称Dynamic properties(动态属性)。
- （7） 图片预加载，将样式表放在顶部，将脚本放在底部  加上时间戳。
## 什么叫优雅降级和渐进增强？
### 优雅降级：
Web站点在所有新式浏览器中都能正常工作，如果用户使用的是老式浏览器，则代码会检查以确认它们是否能正常工作。由于IE独特的盒模型布局问题，针对不同版本的IE的hack实践过优雅降级了,为那些无法支持功能的浏览器增加候选方案，使之在旧式浏览器上以某种形式降级体验却不至于完全失效.

### 渐进增强：
从被所有浏览器支持的基本功能开始，逐步地添加那些只有新式浏览器才支持的功能,向页面增加无害于基础浏览器的额外样式和功能的。当浏览器支持时，它们会自动地呈现出来并发挥作用。

## 哪些常见操作会造成内存泄漏？

内存泄漏：指任何对象在您不再拥有或需要它之后仍然存在。
- 垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。
- setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。
- 闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）
## 一个页面从输入 URL 到页面加载显示完成，这个过程中都发生了什么？

###【http 请求阶段】；
- 1. 浏览器首先会把url发送给DNS服务器；解析出一个服务器的IP地址；
- 2. DNS 服务器会根据IP找到对应的服务器，（服务器需要联网）
- 3.服务器接收到请求；客户端和服务器已经产生了连接；

### 【http的响应阶段】
- 4.服务器接收到请求后，会根据你的传过来的地址，路径，等找到相应的项目；
- 5.在服务器找到之后，服务器立即把一些响应信息放在响应头中，通过http发送给客户端；同时，进行数据的整理；
- 6.把整理出来的数据，通过http发送给客户端；直到客户端数据接收完毕；

### 【浏览器渲染阶段】
- 7.浏览器拿到从服务器传输过来的数据文件；
- 8.首先会遍历HTML，形成DOM树；
- 9.代码从上到下解析，形成css树；
- 10.DOM树和cSS树，重新组合成render树；
- 11.浏览器进行描绘和渲染；

从浏览器发送请求开始，并通过http把数据传输给服务器，服务器通过http把数据返回给客户端，这样一个闭合的过程称为一个http事物；

框架 ：组件化开发 、虚拟DOM；操作的不是真实的DOM
jquery : 真实的DOM。性能慢，比较低；

用户体验 ：
http的三次握手和四次挥手：
浏览器在给服务器传输数据之间，有三次握手，握成功之后，才可以传输数据；

### 三次握手

- 1. 浏览器需要先发送SYN码，客户端请求和服务器建立连接；
- 2. 服务器接收到SYN码，再发送给客户端ACK码，我可以建立连接；
- 3. 客户端接收到ACK码，验证这个ACK是否正确，如果正确，则客户端和服务器就建立起数据连接；双方的数据发送通道都将开启；

### 四次挥手：
- 1.当客户端把数据都发送给服务器，没有数据再传输给服务器，那么会发送一个FIN码
- 2.当服务器接收客户端数据完毕之后，告诉给客户端，给客户端发送AC码，你可以把数据通道关闭；
- 3. 当服务器发送完毕之后，也会发送FIN码，告诉浏览器，数据发送完毕
- 4.当客户端接收完毕之后，同样发送ACK码，告诉服务器，数据接收完毕，你可以进行关闭；

#### 优点：
- 1. 确保数据的安全性；
- 2. 确保数据的完整性；
- 响应头：服务器会告诉浏览器数据的长度；浏览器接收的数据长度和响应头数据长度相同，说明数据已经接收完毕；

## 如何将浮点数点左边的数每三位添加一个逗号，如12000000.11转化为『12,000,000.11』?

```javascript
 function commafy(num){
  	return num && num
  		.toString()
  		.replace(/(\d)(?=(\d{3})+\.)/g, function($1, $2){
  			return $2 + ',';
  		});
}
```

## 如何实现数组的随机排序？

```javascript
方法一：
  	var arr = [1,2,3,4,5,6,7,8,9,10];
  	function randSort1(arr){
  		for(var i = 0,len = arr.length;i < len; i++ ){
  			var rand = parseInt(Math.random()*len);
  			var temp = arr[rand];
  			arr[rand] = arr[i];
  			arr[i] = temp;
  		}
  		return arr;
  	}
  	console.log(randSort1(arr));
  	
  方法二：
  	var arr = [1,2,3,4,5,6,7,8,9,10];
  	function randSort2(arr){
  		var mixedArray = [];
  		while(arr.length > 0){
  			var randomIndex = parseInt(Math.random()*arr.length);
  			mixedArray.push(arr[randomIndex]);
  			arr.splice(randomIndex, 1);
  		}
  		return mixedArray;
  	}
  	console.log(randSort2(arr));

  方法三：
  	var arr = [1,2,3,4,5,6,7,8,9,10];
  	arr.sort(function(){
  		return Math.random() - 0.5;
  	})
  	console.log(arr);
```

## Javascript如何实现继承？

### 继承方式
#### 原型式继承

核心：将父类的实例作为子类的原型。

```javascript
SubType.prototype = newSuperType() 
// 所有涉及到原型链继承的继承方式都要修改子类构造函数的指向，否则子类实例的构造函数会指向SuperType。
SubType.prototype.constructor = SubType;
```

##### 优点：父类方法可以复用。

##### 缺点：

- 父类的引用属性会被所有子类实例共享
- 子类构建实例时不能向父类传递参数

### 构造函数继承

核心：将父类构造函数的内容复制给了子类的构造函数。这是所有继承中唯一一个不涉及到prototype的继承。

```javascript
SuperType.call(SubType);
```

##### 优点：和原型链继承完全反过来

- 父类的引用属性不会被共享

- 子类构建实例时可以向父类传递参数

##### 缺点：父类的方法不能复用，子类实例的方法每次都是单独创建的。

#### 组合继承

核心：原型式继承和构造函数继承的组合，兼具了二者的优点。

```javascript
functionSuperType() {   
this.name = 'parent';
this.arr = [1, 2, 3];
}
SuperType.prototype.say = function() { 
    console.log('this is parent')
}
functionSubType() {
SuperType.call(this) 
// 第二次调用SuperType
}
SubType.prototype = newSuperType() 
// 第一次调用SuperType
```

##### 优点：

- 父类的方法可以被复用

- 父类的引用属性不会被共享

- 子类构建实例时可以向父类传递参数

##### 缺点：
调用了两次父类的构造函数，第一次给子类的原型添加了父类的name, arr属性，第二次又给子类的构造函数添加了父类的name, arr属性，从而覆盖了子类原型中的同名参数。这种被覆盖的情况造成了性能上的浪费。

#### 原型式继承
核心：原型式继承的object方法本质上是对参数对象的一个浅复制。

```javascript
functionobject(o){
    function F(){}
    F.prototype = o;
return new F();
}
var person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
};
var anotherPerson = object(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");
var yetAnotherPerson = object(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.push("Barbie");
alert(person.friends);   
//"Shelby,Court,Van,Rob,Barbie"
```

ECMAScript 5 通过新增 Object.create()方法规范化了原型式继承。这个方法接收两个参数:一 个用作新对象原型的对象和(可选的)一个为新对象定义额外属性的对象。在传入一个参数的情况下， Object.create()与 object()方法的行为相同。——《JAVASCript高级编程》

所以上文中代码可以转变为：

```javascript
var yetAnotherPerson = object(person); => 
var yetAnotherPerson = Object.create(person);
```

##### 优点：父类方法可以复用。

##### 缺点：

- 父类的引用属性会被所有子类实例共享

- 子类构建实例时不能向父类传递参数 

#### 寄生式继承

核心：使用原型式继承获得一个目标对象的浅复制，然后增强这个浅复制的能力。

##### 优缺点：仅提供一种思路，没什么优点。

```javascript
function createAnother(original){ 
    var clone = object(original);    
//通过调用函数创建一个新对象
    clone.sayHi = function(){      
//以某种方式来增强这个对象
        alert("hi");
};    
return clone;                  
//返回这个对象
}
var person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
};
var anotherPerson = createAnother(person);
anotherPerson.sayHi(); 
//"hi"
```

#### 寄生组合继承

刚才说到组合继承有一个会两次调用父类的构造函数造成浪费的缺点，寄生组合继承就可以解决这个问题。

```javascript
function inheritPrototype(subType, superType){
    var prototype = object(superType.prototype); 
// 创建了父类原型的浅复制
    prototype.constructor = subType;             
// 修正原型的构造函数
    subType.prototype = prototype;               
// 将子类的原型替换为这个原型
}
function SuperType(name){
    this.name = name;
    this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function(){
    alert(this.name);
};
function SubType(name, age){
    SuperType.call(this, name);
    this.age = age;
}
// 核心：因为是对父类原型的复制，所以不包含父类的构造函数，也就不会调用两次父类的构造函数造成浪费
inheritPrototype(SubType, SuperType);
SubType.prototype.sayAge = function(){
    alert(this.age);
}
```

##### 优缺点：这是一种完美的继承方式。

#### ES6的类继承
核心： ES6继承的结果和寄生组合继承相似，本质上，ES6继承是一种语法糖。但是，寄生组合继承是先创建子类实例this对象，然后再对其增强；而ES6先将父类实例对象的属性和方法，加到this上面（所以必须先调用super方法），然后再用子类的构造函数修改this。

```javascript
class A {}
class B extends A {
  constructor() {
    super();
  }
}
```

ES6实现继承的具体原理：

```javascript
class A {}
class B {}
Object.setPrototypeOf = function(obj, proto) {
  obj.__proto__ = proto;
return obj;
}
// B 的实例继承 A 的实例
Object.setPrototypeOf(B.prototype, A.prototype);
// B 继承 A 的静态属性
Object.setPrototypeOf(B, A);
```

##### ES6继承与ES5继承的异同：
###### 相同点：本质上ES6继承是ES5继承的语法糖。
###### 不同点：

- ES6继承中子类的构造函数的原型链指向父类的构造函数，ES5中使用的是构造函数复制，没有原型链指向。
- ES6子类实例的构建，基于父类实例，ES5中不是。

#### 总结

- ES6 Class extends是ES5继承的语法糖
- JS的继承除了构造函数继承之外都基于原型链构建的
- 可以用寄生组合继承实现ES6 Class extends，但是还是会有细微的差别

## javascript创建对象的几种方式？

javascript创建对象简单的说,无非就是使用内置对象或各种自定义对象，当然还可以用JSON；但写法有很多种，也能混合使用。

### 1、对象字面量的方式

```javascript
person={firstname:"Mark",lastname:"Yun",age:25,eyecolor:"black"};
```

### 2、用function来模拟无参的构造函数

```javascript
function Person(){}
 	var person=new Person();//定义一个function，如果使用new"实例化",该function可以看作是一个Class
 	person.name="Mark";
 	person.age="25";
 	person.work=function(){
 	alert(person.name+" hello...");
}
person.work();
```

### 3、用function来模拟参构造函数来实现（用this关键字定义构造的上下文属性）

```javascript
function Pet(name,age,hobby){
 	   this.name=name;//this作用域：当前对象
 	   this.age=age;
 	   this.hobby=hobby;
 	   this.eat=function(){
 	      alert("我叫"+this.name+",我喜欢"+this.hobby+",是个程序员");
 	}
}
var maidou =new Pet("麦兜",25,"coding");//实例化、创建对象
maidou.eat();//调用eat方法
```

### 4、用工厂方式来创建（内置对象）

```javascript
var wcDog =new Object();
 	wcDog.name="旺财";
 	wcDog.age=3;
 	wcDog.work=function(){
 	   alert("我是"+wcDog.name+",汪汪汪......");
 	}
wcDog.work();
```

### 5、用原型方式来创建

```javascript
function Dog(){}
Dog.prototype.name="旺财";
Dog.prototype.eat=function(){
    alert(this.name+"是个吃货");
}
var wangcai =new Dog();
wangcai.eat();
```

### 6、用混合方式来创建

```javascript
function Car(name,price){
 	  this.name=name;
 	  this.price=price;
}
Car.prototype.sell=function(){
 	alert("我是"+this.name+"，我现在卖"+this.price+"万元");
}
var camry =new Car("凯美瑞",27);
 	camry.sell();
```

## 什么是window对象? 什么是document对象?
- window对象是指浏览器打开的窗口。
- document对象是Documentd对象（HTML 文档对象）的一个只读引用，window对象的一个属性。

## 写一个通用的事件侦听器函数。

```javascript
// event(事件)工具集，来源：github.com/markyun
 	markyun.Event = {
 		// 页面加载完成后
 		readyEvent : function(fn) {
 			if (fn==null) {
 				fn=document;
 			}
 			var oldonload = window.onload;
 			if (typeof window.onload != 'function') {
 				window.onload = fn;
 			} else {
 				window.onload = function() {
 					oldonload();
 					fn();
 				};
 			}
 		},
 		// 视能力分别使用dom0||dom2||IE方式 来绑定事件
 		// 参数： 操作的元素,事件名称 ,事件处理程序
 		addEvent : function(element, type, handler) {
 			if (element.addEventListener) {
 				//事件类型、需要执行的函数、是否捕捉
 				element.addEventListener(type, handler, false);
 			} else if (element.attachEvent) {
 				element.attachEvent('on' + type, function() {
 					handler.call(element);
 				});
 			} else {
 				element['on' + type] = handler;
 			}
 		},
 		// 移除事件
 		removeEvent : function(element, type, handler) {
 			if (element.removeEventListener) {
 				element.removeEventListener(type, handler, false);
 			} else if (element.datachEvent) {
 				element.detachEvent('on' + type, handler);
 			} else {
 				element['on' + type] = null;
 			}
 		},
 		// 阻止事件 (主要是事件冒泡，因为IE不支持事件捕获)
 		stopPropagation : function(ev) {
 			if (ev.stopPropagation) {
 				ev.stopPropagation();
 			} else {
 				ev.cancelBubble = true;
 			}
 		},
 		// 取消事件的默认行为
 		preventDefault : function(event) {
 			if (event.preventDefault) {
 				event.preventDefault();
 			} else {
 				event.returnValue = false;
 			}
 		},
 		// 获取事件目标
 		getTarget : function(event) {
 			return event.target || event.srcElement;
 		},
 		// 获取event对象的引用，取到事件的所有信息，确保随时能使用event；
 		getEvent : function(e) {
 			var ev = e || window.event;
 			if (!ev) {
 				var c = this.getEvent.caller;
 				while (c) {
 					ev = c.arguments[0];
 					if (ev && Event == ev.constructor) {
 						break;
 					}
 					c = c.caller;
 				}
 			}
 			return ev;
 		}
 	};
```

## ["1", "2", "3"].map(parseInt) 答案是多少？

```javascript
parseInt() 函数能解析一个字符串，并返回一个整数，需要两个参数 (val, radix)，
 其中 radix 表示要解析的数字的基数。【该值介于 2 ~ 36 之间，并且字符串中的数字不能大于radix才能正确返回数字结果值】;
 但此处 map 传了 3 个 (element, index, array),我们重写parseInt函数测试一下是否符合上面的规则。

 function parseInt(str, radix) {
     return str+'-'+radix;
 };
 var a=["1", "2", "3"];
 a.map(parseInt);  // ["1-0", "2-1", "3-2"] 不能大于radix

 因为二进制里面，没有数字3,导致出现超范围的radix赋值和不合法的进制解析，才会返回NaN
 所以["1", "2", "3"].map(parseInt) 答案也就是：[1, NaN, NaN]
```

## 事件是？IE与火狐的事件机制有什么区别？ 如何阻止冒泡？

- 1. 我们在网页中的某个操作（有的操作对应多个事件）。例如：当我们点击一个按钮就会产生一个事件。是可以被 JavaScript 侦测到的行为。
- 2. 事件处理机制：IE是事件冒泡、Firefox同时支持两种事件模型，也就是：捕获型事件和冒泡型事件；
- 3. ev.stopPropagation();(旧ie的方法 ev.cancelBubble = true;)

## 如何解决跨域问题？

要掌握跨域，首先要知道为什么会有跨域这个问题出现？

### 为什么会有跨域
由于浏览器存在同源策略，请求的 Url 地址的协议、主机名、端口号必须完全相同，否则会产生跨域，同源策略的限制下 cookie 、loclstorage、dom、ajax、IndexDB 等都不允许跨域、form 表单不受同源策略限制
对跨域的理解有一个误区，跨域不是请求没有发送出去或者服务器接收到请求而没有响应，正确的情况是请求发出，服务器响应，由于响应和请求来自不同的域被浏览器拦截了。

### 没有同源策略限制的两大危险场景
浏览器是从两个方面去做这个同源策略的，
一是针对接口的请求，二是针对Dom的查询。

### 没有同源策略限制的接口请求

有一个小小的东西叫cookie大家应该知道，一般用来处理登录等场景，目的是让服务端知道谁发出的这次请求。如果你请求了接口进行登录，服务端验证通过后会在响应头加入Set-Cookie字段，然后下次再发请求的时候，浏览器会自动将cookie附加在HTTP请求的头字段Cookie中，服务端就能知道这个用户已经登录过了。知道这个之后，我们来看场景：

- 你准备去清空你的购物车，于是打开了买买买网站www.maimaimai.com，然后登录成功，一看，购物车东西这么少，不行，还得买多点。

- 你在看有什么东西买的过程中，你的好基友发给你一个链接www.nidongde.com，一脸yin笑地跟你说：“你懂的”，你毫不犹豫打开了。

- 你饶有兴致地浏览着www.nidongde.com，谁知这个网站暗地里做了些不可描述的事情！由于没有同源策略的限制，它向www.maimaimai.com发起了请求！聪明的你一定想到上面的话“服务端验证通过后会在响应头加入Set-Cookie字段，然后下次再发请求的时候，浏览器会自动将cookie附加在HTTP请求的头字段Cookie中”，这样一来，这个不法网站就相当于登录了你的账号，可以为所欲为了！如果这不是一个买买买账号，而是你的银行账号，那……

这就是传说中的CSRF攻击。

### 没有同源策略限制的Dom查询
- 有一天你刚睡醒，收到一封邮件，说是你的银行账号有风险，赶紧点进www.yinghang.com改密码。你吓尿了，赶紧点进去，还是熟悉的银行登录界面，你果断输入你的账号密码，登录进去看看钱有没有少了。

- 睡眼朦胧的你没看清楚，平时访问的银行网站是www.yinhang.com，而现在访问的是www.yinghang.com，这个钓鱼网站做了什么呢？

```javascript
// HTML
<iframe name="yinhang" src="www.yinhang.com"></iframe>
// JS
// 由于没有同源策略的限制，钓鱼网站可以直接拿到别的网站的Dom
const iframe = window.frames['yinhang']
const node = iframe.document.getElementById('你输入账号密码Input')
console.log(`拿到了这个${node}，我还拿不到你刚刚输入的账号密码吗`)
```

由此我们知道，同源策略确实能规避一些危险，不是说有了同源策略就安全，只是说同源策略是一种浏览器最基本的安全机制，毕竟能提高一点攻击的成本。其实没有刺不穿的盾，只是攻击的成本和攻击成功后获得的利益成不成正比。

### 跨域正确的打开方式
经过对同源策略的了解，我们应该要消除对浏览器的误解，同源策略是浏览器做的一件好事，是用来防御来自邪门歪道的攻击，但总不能为了不让坏人进门而把全部人都拒之门外吧。没错，我们这种正人君子只要打开方式正确，就应该可以跨域。

下面将一个个演示正确打开方式，但在此之前，有些准备工作要做。为了本地演示跨域，我们需要：

随便跑起一份前端代码（以下前端是随便跑起来的vue），地址是http://localhost:9099。

随便跑起一份后端代码（以下后端是随便跑起来的node koa2），地址是http://localhost:9971。

### 同源策略限制下接口请求的正确打开方式
#### JSONP

在HTML标签里，一些标签比如script、img这样的获取资源的标签是没有跨域限制的，利用这一点，我们可以这样干。

后端写个小接口：

```javascript
// 处理成功失败返回格式的工具
const {successBody} = require('../utli')
class CrossDomain{
    static async jsonp (ctx) {
// 前端传过来的参数
const query = ctx.request.query
// 设置一个cookies
    ctx.cookies.set('tokenId', '1')
// query.cb是前后端约定的方法名字，其实就是后端返回一个直接执行的方法给前端，由于前端是用script标签发起的请求，所以返回了这个方法后相当于立马执行，并且把要返回的数据放在方法的参数里。
    ctx.body = `${query.cb}(${JSON.stringify(successBody({msg: query.msg}, 'success'))})`
}
}
module.exports = CrossDomain
```

简单版前端：

```javascript
<!DOCTYPE html>
<html>
<head>   
<meta charset="utf-8">
</head>
<body>
<script type='text/javascript'>
// 后端返回直接执行的方法，相当于执行这个方法，由于后端把返回的数据放在方法的参数里，所以这里能拿到res。
      window.jsonpCb = function(res) {
        console.log(res)
      }
</script>
<script src='http://localhost:9871/api/jsonp?msg=helloJsonp&cb=jsonpCb' type='text/javascript'></script>
</body>
</html>
```

简单封装一下前端这个套路：

```javascript
/**
 * JSONP请求工具
 * @param url 请求的地址
 * @param data 请求的参数
 * @returns {Promise<any>}
 */
const request = ({url, data}) => {
    return new Promise((resolve, reject) => {
// 处理传参成xx=yy&aa=bb的形式
const handleData = (data) => {
const keys = Object.keys(data)
const keysLen = keys.length
    return keys.reduce((pre, cur, index) => {
const value = data[cur]  
const flag = index !== keysLen - 1 ? '&' : ''
return `${pre}${cur}=${value}${flag}`
}, '')
}    
// 动态创建script标签   
const script = document.createElement('script')
// 接口返回的数据获取
    window.jsonpCb = (res) => {
      document.body.removeChild(script)
    delete window.jsonpCb
      resolve(res)
}
    script.src = `${url}?${handleData(data)}&cb=jsonpCb`
    document.body.appendChild(script)
})
}
// 使用方式
request({
  url: 
'http://localhost:9871/api/jsonp',
  data: {
// 传参
    msg: 'helloJsonp'
}
}).then(res => {
  console.log(res)
})
```

#### 空iframe加form
细心的朋友可能发现，JSONP只能发GET请求，因为本质上script加载资源就是GET，那么如果要发POST请求怎么办呢？

后端写个小接口：

```javascript
// 处理成功失败返回格式的工具
const {successBody} = require('../utli')
class CrossDomain{
    static async iframePost (ctx) {
        let postData = ctx.request.body
        console.log(postData)
        ctx.body = successBody({postData: postData}, 'success')
}
}
module.exports = CrossDomain
```

前端：

```javascript
const requestPost = ({url, data}) => {
// 首先创建一个用来发送数据的iframe.
const iframe = document.createElement('iframe')
iframe.name = 'iframePost'
iframe.style.display = 'none'
document.body.appendChild(iframe)
const form = document.createElement('form')
const node = document.createElement('input')
// 注册iframe的load事件处理程序,如果你需要在响应返回时执行一些操作的话.
iframe.addEventListener('load', function() {
    console.log('post success')
})
form.action = url  
// 在指定的iframe中执行form
form.target = iframe.name
form.method = 'post'
for(let name indata) {
    node.name = name
    node.value = data[name].toString()
    form.appendChild(node.cloneNode())
} 
// 表单元素需要添加到主文档中.
form.style.display = 'none'
document.body.appendChild(form)
form.submit()
// 表单提交后,就可以删除这个表单,不影响下次的数据发送.
document.body.removeChild(form)
}
// 使用方式
requestPost({
  url: 
'http://localhost:9871/api/iframePost',
  data: {
    msg: 'helloIframePost'
  }
})
```

#### CORS
CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）跨域资源共享 CORS 详解。看名字就知道这是处理跨域问题的标准做法。CORS有两种请求，简单请求和非简单请求。

浏览器将CORS请求分成两类：简单请求（simple request）和非简单请求（not-so-simple request）。
只要同时满足以下两大条件，就属于简单请求

（1）请求方法是以下三种方法之一：
- HEAD
- GET
- POST

（2）HTTP的头信息不超出以下几种字段：
- Accept
- Accept-Language
- Content-Language
- Last-Event-ID
- Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain

##### 简单请求
后端：

```javascript
// 处理成功失败返回格式的工具
const {successBody} = require('../utli')
class CrossDomain{
    static async cors (ctx) { 
    const query = ctx.request.query   
// *时cookie不会在http请求中带上
    ctx.set('Access-Control-Allow-Origin', '*')
    ctx.cookies.set('tokenId', '2')
    ctx.body = successBody({msg: query.msg}, 'success')
  }
}
module.exports = CrossDomain
```

前端什么也不用干，就是正常发请求就可以，如果需要带cookie的话，前后端都要设置一下，下面那个非简单请求例子会看到。

```javascript
fetch(`http://localhost:9871/api/cors?msg=helloCors`).then(res => {console.log(res)})
```

##### 非简单请求
非简单请求会发出一次预检测请求，返回码是204，预检测通过才会真正发出请求，这才返回200。这里通过前端发请求的时候增加一个额外的headers来触发非简单请求。

后端：

```javascript
// 处理成功失败返回格式的工具
const {successBody} = require('../utli')
class CrossDomain{
    static async cors (ctx) {
const query = ctx.request.query
// 如果需要http请求中带上cookie，需要前后端都设置credentials，且后端设置指定的origin
    ctx.set('Access-Control-Allow-Origin','http://localhost:9099')
    ctx.set('Access-Control-Allow-Credentials', true)
// 非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求（preflight）
// 这种情况下除了设置origin，还需要设置Access-Control-Request-Method以及Access-Control-Request-Headers
    ctx.set('Access-Control-Request-Method', 'PUT,POST,GET,DELETE,OPTIONS')
    ctx.set('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-Type, Accept, t')
    ctx.cookies.set('tokenId', '2')
    ctx.body = successBody({msg: query.msg}, 'success')
}
}
module.exports = CrossDomain
```

一个接口就要写这么多代码，如果想所有接口都统一处理，有什么更优雅的方式呢？见下面的koa2-cors。

```javascript
const path = require('path')
const Koa = require('koa')
const koaStatic = require('koa-static')
const bodyParser = require('koa-bodyparser')
const router = require('./router')
const cors = require('koa2-cors')
const app = new Koa()
const port = 9871
app.use(bodyParser())
// 处理静态资源 这里是前端build好之后的目录
app.use(koaStatic(path.resolve(__dirname, '../dist')))
// 处理cors
app.use(cors({
  origin: function(ctx) {
      return 'http://localhost:9099'
    },
  credentials: true,
  allowMethods: ['GET', 'POST', 'DELETE'],
  allowHeaders: ['t', 'Content-Type']
}))
// 路由
app.use(router.routes()).use(router.allowedMethods())
// 监听端口
app.listen(9871)
console.log(`[demo] start-quick is starting at port ${port}`)
```

前端：

```javascript
fetch(`http://localhost:9871/api/cors?msg=helloCors`, {
// 需要带上cookie
  credentials: 'include',
// 这里添加额外的headers来触发非简单请求
  headers: {
      't': 'extra headers'
  }
}).then(res => {
  console.log(res)
})
```

#### 代理
想一下，如果我们请求的时候还是用前端的域名，然后有个东西帮我们把这个请求转发到真正的后端域名上，不就避免跨域了吗？这时候，Nginx出场了。

Nginx配置：

```javascript
server{
# 监听9099端口
    listen 9099;
# 域名是localhost
    server_name localhost;
#凡是localhost:9099/api这个样子的，都转发到真正的服务端地址http://localhost:9871 
    location ^~ /api {
        proxy_pass http:
//localhost:9871;
    }   
}
```

前端就不用干什么事情了，除了写接口，也没后端什么事情了。

```javascript
// 请求的时候直接用回前端这边的域名http://localhost:9099，这就不会跨域，然后Nginx监听到凡是localhost:9099/api这个样子的，都转发到真正的服务端地址http://localhost:9871 
fetch('http://localhost:9099/api/iframePost', {
  method: 'POST',
  headers: {
      'Accept': 'application/json',
      'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    msg: 'helloIframePost'
  })
})
```

Nginx转发的方式似乎很方便！但这种使用也是看场景的，如果后端接口是一个公共的API，比如一些公共服务获取天气什么的，前端调用的时候总不能让运维去配置一下Nginx，如果兼容性没问题（IE 10或者以上），CROS才是更通用的做法吧。

### 同源策略限制下Dom查询的正确打开方式

#### postMessage

window.postMessage() 是HTML5的一个接口，专注实现不同窗口不同页面的跨域通讯。

为了演示方便，我们将hosts改一下：127.0.0.1 crossDomain.com，现在访问域名crossDomain.com就等于访问127.0.0.1。

这里是http://localhost:9099/#/crossDomain，发消息方：

```javascript
<template> 
<div>
<button @click = "postMessage">
//给http://crossDomain.com:9099发消息
</button>    
<iframe name = "crossDomainIframe" src = "http://crossdomain.com:9099"></iframe>
</div>
</template>
<script>
    export default{
        mounted () {
        window.addEventListener('message', (e) => {
// 这里一定要对来源做校验
        if(e.origin === 'http://crossdomain.com:9099') {
// 来自http://crossdomain.com:9099的结果回复
        console.log(e.data)
      }
    })
  },
  methods: {
// 向http://crossdomain.com:9099发消息
    postMessage () {
    const iframe = window.frames['crossDomainIframe']
      iframe.postMessage('我是[http://localhost:9099], 麻烦你查一下你那边有没有id为app的Dom', 'http://crossdomain.com:9099')
    }
  }
}
</script>
```

这里是http://crossdomain.com:9099，接收消息方：

```javascript
<template>
<div>
    我是http://crossdomain.com:9099
</div>
</template>
<script>
export default
 {
  mounted () {
    window.addEventListener('message', (e) => {
// 这里一定要对来源做校验
if(e.origin === 'http://localhost:9099') {
// http://localhost:9099发来的信息
        console.log(e.data);        
// e.source可以是回信的对象，其实就是http://localhost:9099窗口对象(window)的引用
// e.origin可以作为targetOrigin
        e.source.postMessage(`我是[http:
//crossdomain.com:9099]，我知道了兄弟，这就是你想知道的结果：${document.getElementById('app') ? '有id为app的Dom' : '没有id为app的Dom'}`, e.origin);
      }
    })
  }
}
</script>
```

#### document.domain

这种方式只适合主域名相同，但子域名不同的iframe跨域。

比如主域名是http://crossdomain.com:9099，子域名是http://child.crossdomain.com:9099，这种情况下给两个页面指定一下document.domain即document.domain = crossdomain.com就可以访问各自的window对象了。

#### canvas操作图片的跨域问题

## documen.write和 innerHTML的区别
- document.write只能重绘整个页面
- innerHTML可以重绘页面的一部分

## DOM操作——怎样添加、移除、移动、复制、创建和查找节点?

### （1）创建新节点
- createDocumentFragment()    //创建一个DOM片段
- createElement()   //创建一个具体的元素
- createTextNode()   //创建一个文本节点
###（2）添加、移除、替换、插入
- appendChild()
- removeChild()
- replaceChild()
- insertBefore() //在已有的子节点前插入一个新的子节点
###（3）查找
- getElementsByTagName()    //通过标签名称
- getElementsByName()    //通过元素的Name属性的值(IE容错能力较强，会得到一个数组，其中包括id等于name值的)
- getElementById()    //通过元素Id，唯一性

## jquery.extend 与 jquery.fn.extend的区别？
jquery.extend 为jquery类添加类方法，可以理解为添加静态方法
jquery.fn.extend:
- 源码中jquery.fn = jquery.prototype，所以对jquery.fn的扩展，就是为jquery类添加成员函数

使用：
- jquery.extend扩展，需要通过jquery类来调用，而jquery.fn.extend扩展，所有jquery实例都可以直接调用。

## 那些操作会造成内存泄漏？

内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。

垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。

setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。
 闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）

## JQuery一个对象可以同时绑定多个事件，这是如何实现的？

```javascript
多个事件同一个函数：
 	$("div").on("click mouseover", function(){});
多个事件不同函数
 	$("div").on({
 		click: function(){},
 		mouseover: function(){}
 	});
```

## 用js实现千位分隔符?

```javascript
function commafy(num) {
      return num && num
          .toString()
          .replace(/(\d)(?=(\d{3})+\.)/g, function($0, $1) {
              return $1 + ",";
          });
  }
console.log(commafy(1234567.90)); //1,234,567.90
```

## 检测浏览器版本版本有哪些方式？

功能检测、userAgent特征检测

比如：navigator.userAgent
//"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_2) AppleWebKit/537.36
(KHTML, like Gecko) Chrome/41.0.2272.101 Safari/537.36"

## 使用JS实现获取文件扩展名？

```javascript
function getFileExtension(filename) {
    return filename.slice((filename.lastIndexOf(".") - 1 >>> 0) + 2);
  }

String.lastIndexOf() 方法返回指定值（本例中的'.'）在调用该方法的字符串中最后出现的位置，如果没找到则返回 -1。
对于'filename'和'.hiddenfile'，lastIndexOf的返回值分别为0和-1无符号右移操作符(»>) 将-1转换为4294967295，将-2转换为4294967294，这个方法可以保证边缘情况时文件名不变。
String.prototype.slice() 从上面计算的索引处提取文件的扩展名。如果索引比文件名的长度大，结果为""。
```

## Webpack热更新实现原理?

- 1. Webpack编译期，为需要热更新的 entry 注入热更新代码(EventSource通信)
- 2. 页面首次打开后，服务端与客户端通过 EventSource 建立通信渠道，把下一次的 hash 返回前端
- 3. 客户端获取到hash，这个hash将作为下一次请求服务端 hot-update.js 和 hot-update.json的hash
- 4. 修改页面代码后，Webpack 监听到文件修改后，开始编译，编译完成后，发送 build 消息给客户端
- 5. 客户端获取到hash，成功后客户端构造hot-update.js script链接，然后插入主文档
- 6. hot-update.js 插入成功后，执行hotAPI 的 createRecord 和 reload方法，获取到 Vue 组件的 render方法，重新 render 组件， 继而实现 UI 无刷新更新。

## 描述一下React 生命周期

渲染过程调用到的生命周期函数，主要几个要知道；

- constructor
- getInitialState
- getDefaultProps
- componentWillMount
- render
- componentDidMount

  	更新过程

- componentWillReceiveProps
- shouldComponentUpdate
- componentWillUpdate
- render
- componentDidUpdate

  	卸载过程
  	
- componentWillUnmount

## 实现组件有哪些方式？

- React.createClass 使用API来定义组件
- React ES6 class component 用 ES6 的class 来定义组件
- Functional stateless component 通过函数定义无状态组件

## 应该在React生命周期的什么阶段发出ajax请求，为什么？

AJAX请求应在 componentDidMount函数 进行请求。

## shouldComponentUpdate函数有什么作用？

shouldComponentUpdate是一个允许我们自行决定某些组件（以及他们的子组件）是否进行更新的生命周期函数，reconciliation的最终目的是尽可能以最有效的方式去根据新的state更新UI，

如果你已经知道UI的哪些状态无需进行改变，就没必要去让React去判断它是否该改变。 让shouldComponentUpdate返回falss, React就会让当前的组件和其子组件保持不变。

## 当组件的setState函数被调用之后，发生了什么？

React会做的第一件事就是把你传递给setState的参数对象合并到组件原先的state。这个事件会导致一个“reconciliation”（调和）的过程。reconciliation的最终目标就是，

尽可能以最高效的方法，去基于新的state来更新UI。为了达到这个目的，React会构建一个React元素树（你可以把这个想象成一个表示UI的一个对象）。一旦这个树构建完毕，

React为了根据新的state去决定UI要怎么进行改变，它会找出这棵新树和旧树的不同之处。React能够相对精确地找出哪些位置发生了改变以及如何发生了什么变化，
  		
并且知道如何只通过必要的更新来最小化重渲染。

## 为什么循环产生的组件中要利用上key这个特殊的prop？
Keys负责帮助React跟踪列表中哪些元素被改变/添加/移除。React利用子元素的key在比较两棵树的时候，快速得知一个元素是新的还是刚刚被移除。没有keys，React也就不知道当前哪一个的item被移除了。


## React-router 路由的实现原理？

## 说说React Native,Weex框架的实现原理？

## 受控组件(Controlled Component)与非受控组件(Uncontrolled Component)的区别

## refs 是什么?
  	Refs是能访问DOM元素或组件实例的一个函数；

## React为什么自己定义一套事件体系呢，与浏览器原生事件体系有什么关系？
## 什么时候应该选择用class实现一个组件，什么时候用一个函数实现一个组件？
组件用到了state或者用了生命周期函数，那么就该使用Class component。其他情况下，应使用Functional component。

## 什么是HoC（Higher-Order Component）？适用于什么场景？

高阶组件就是一个 React 组件包裹着另外一个 React 组件
并不是父子关系的组件，如何实现相互的数据通信？

使用父组件，通过props将变量传入子组件 （如通过refs，父组件获取一个子组件的方法，简单包装后，将包装后的方法通过props传入另一个子组件）

## 用过 React 技术栈中哪些数据流管理库？
  	Redux\Dva
## Redux是如何做到可预测呢？

## Redux将React组件划分为哪两种？

## Redux是如何将state注入到React组件上的？

## 请描述一次完整的 Redux 数据流

## React的批量更新机制 BatchUpdates？

## React与Vue，各自的组件更新进行对比，它们有哪些区别？

## 你遇到过比较难的技术问题是？你是如何解决的？

## 设计模式 知道什么是singleton, factory, strategy, decrator么?

## 常使用的库有哪些？常用的前端开发工具？开发过什么应用或组件？

## 页面重构怎么操作？

- 网站重构：在不改变外部行为的前提下，简化结构、添加可读性，而在网站前端保持一致的行为。

也就是说是在不改变UI的情况下，对网站进行优化，在扩展的同时保持一致的UI。

## 对于传统的网站来说重构通常是：

- 表格(table)布局改为DIV+CSS
- 使网站前端兼容于现代浏览器(针对于不合规范的CSS、如对IE6有效的)
- 对于移动平台的优化
- 针对于SEO进行优化
- 深层次的网站重构应该考虑的方面

- 减少代码间的耦合
- 让代码保持弹性
- 严格按规范编写代码
- 设计可扩展的API
- 代替旧有的框架、语言(如VB)
- 增强用户体验
- 通常来说对于速度的优化也包含在重构中

- 压缩JS、CSS、image等前端资源(通常是由服务器来解决)
程序的性能优化(如数据读写)
- 采用CDN来加速资源加载
- 对于JS DOM的优化
- HTTP服务器的文件缓存

## 列举IE与其他浏览器不一样的特性？

### 1、事件不同之处：
- 触发事件的元素被认为是目标（target）。而在 IE 中，目标包含在 event 对象的 srcElement 属性；
- 获取字符代码、如果按键代表一个字符（shift、ctrl、alt除外），IE 的 keyCode 会返回字符代码（Unicode），DOM 中按键的代码和字符是分离的，要获取字符代码，需要使用 charCode 属性；
- 阻止某个事件的默认行为，IE 中阻止某个事件的默认行为，必须将 returnValue 属性设置为 false，Mozilla 中，需要调用 preventDefault() 方法；
- 停止事件冒泡，IE 中阻止事件进一步冒泡，需要设置 cancelBubble 为 true，Mozzilla 中，需要调用 stopPropagation()；

## 99%的网站都需要被重构是那本书上写的？

  - 网站重构：应用web标准进行设计（第2版）

## WEB应用从服务器主动推送Data到客户端有那些方式？

- html5提供的Websocket
- 不可见的iframe
- WebSocket通过Flash
- XHR长时间连接
- XHR Multipart Streaming

## 对Node的优点和缺点提出了自己的看法？

### （优点）因为Node是基于事件驱动和无阻塞的，所以非常适合处理并发请求，

因此构建在Node上的代理服务器相比其他技术实现（如Ruby）的服务器表现要好得多。

此外，与Node代理服务器交互的客户端代码是由javascript语言编写的，

因此客户端和服务器端都用同一种语言编写，这是非常美妙的事情。

### （缺点）Node是一个相对新的开源项目，所以不太稳定，它总是一直在变，
而且缺少足够多的第三方库支持。看起来，就像是Ruby/Rails当年的样子。

## http状态码有那些？分别代表是什么意思？

### 简单版
  	[
  	100  Continue	继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息
  	200  OK 		正常返回信息
  	201  Created  	请求成功并且服务器创建了新的资源
  	202  Accepted 	服务器已接受请求，但尚未处理
  	301  Moved Permanently  请求的网页已永久移动到新位置。
  	302 Found  		临时性重定向。
  	303 See Other  	临时性重定向，且总是使用 GET 请求新的 URI。
  	304  Not Modified 自从上次请求后，请求的网页未修改过。
  	400 Bad Request  服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。
  	401 Unauthorized 请求未授权。
  	403 Forbidden  	禁止访问。
  	404 Not Found  	找不到如何与 URI 相匹配的资源。
  	500 Internal Server Error  最常见的服务器端错误。
  	503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）。
  	]

### 详细版：
- 1、浏览器会开启一个线程来处理这个请求，对 URL 分析判断如果是 http 协议就按照 Web 方式来处理;
- 2、调用浏览器内核中的对应方法，比如 WebView 中的 loadUrl 方法;
- 3、通过DNS解析获取网址的IP地址，设置 UA 等信息发出第二个GET请求;
- 4、进行HTTP协议会话，客户端发送报头(请求报头);
- 5、进入到web服务器上的 Web Server，如 Apache、Tomcat、Node.JS 等服务器;
- 6、进入部署好的后端应用，如 PHP、Java、JavaScript、Python 等，找到对应的请求处理;
- 7、处理结束回馈报头，此处如果浏览器访问过，缓存上有对应资源，会与服务器最后修改时间对比，一致则返回304;
- 8、浏览器开始下载html文档(响应报头，状态码200)，同时使用缓存;
- 9、文档树建立，根据标记请求所需指定MIME类型的文件（比如css、js）,同时设置了cookie;
- 10、页面开始渲染DOM，JS根据DOM API操作DOM,执行事件绑定等，页面显示完成。

### 简洁版：
- 浏览器根据请求的URL交给DNS域名解析，找到真实IP，向服务器发起请求；
- 服务器交给后台处理完成后返回数据，浏览器接收文件（HTML、JS、CSS、图象等）；
- 浏览器对加载到的资源（HTML、JS、CSS等）进行语法解析，建立相应的内部数据结构（如HTML的DOM）；
- 载入解析到的资源文件，渲染页面，完成。
- 部分地区用户反应网站很卡，请问有哪些可能性的原因，以及解决方法？

## 从打开app到刷新出内容，整个过程中都发生了什么，如果感觉慢，怎么定位问题，怎么解决?

## 第一次访问页面中时弹出引导，用户关闭引导，之后再次进入页面时不希望出现引导，如何实现？
  	localStorage

## 除了前端以外还了解什么其它技术么？你最最厉害的技能是什么？

## 你用的得心应手用的熟练地编辑器&开发环境是什么样子？

  Sublime Text 3 + 插件
  Google chrome 查看页面UI、动画效果和交互功能，Firebug 兼容测试和
  Node.js + webpack
  Git 版本控制和Code Review

## 对前端工程师这个职位是怎么样理解的？它的前景会怎么样？

- 前端是最贴近用户的程序员，比后端、数据库、产品经理、运营、安全都近。
- 1、实现界面交互
- 2、提升用户体验
- 3、有了Node.js，前端可以实现服务端的一些事情

前端是最贴近用户的程序员，前端的能力就是能让产品从 90分进化到 100 分，甚至更好，

  参与项目，快速高质量完成实现效果图，精确到1px；

  与团队成员，UI设计，产品经理的沟通；

  做好的页面结构，页面重构和用户体验；

  处理hack，兼容、写出优美的代码格式；

  针对服务器的优化、拥抱最新前端技术。
## 你怎么看待Web App 、hybrid App、Native App？

## 你移动端前端开发的理解？（和 Web 前端开发的主要区别是什么？）

## 产品进行版本升级时，可能发生不兼容性问题，如何提前预防和解决？

  非覆盖式发布，API新增而不是在原来的上面修改；
  提前做好 @Deprecated的版本提示；

## 你对加班的看法？

  加班就像借钱，原则应当是------救急不救穷

## 平时如何管理你的项目？

  先期团队必须确定好全局样式（global.css），编码模式(utf-8) 等；

  编写习惯必须一致（例如都是采用继承式的写法，单样式都写成一行）；

  标注样式编写人，各模块都及时标注（标注关键样式调用的地方）；

  页面进行标注（例如 页面 模块 开始和结束）；

  CSS跟HTML 分文件夹并行存放，命名都得统一（例如style.css）；

  JS 分文件夹存放 命名以该JS功能为准的英文翻译。

  图片采用整合的 images.png png8 格式文件使用 尽量整合在一起使用方便将来的管理

## 如何设计突发大规模并发架构？

## 当团队人手不足，把功能代码写完已经需要加班的情况下，你会做前端代码的测试吗？

## 说说最近最流行的一些东西吧？常去哪些网站？

  	ES6\WebAssembly\Node\MVVM\Web Components\React\React Native\Webpack 组件化

## 知道什么是SEO并且怎么优化么? 知道各种meta data的含义么?

## 移动端（Android IOS）怎么做好用户体验?

  清晰的视觉纵线、
  信息的分组、极致的减法、
  利用选择代替输入、
  标签及文字的排布方式、
  依靠明文确认密码、
  合理的键盘利用、

## 简单描述一下你做过的移动APP项目研发流程？

## 你在现在的团队处于什么样的角色，起到了什么明显的作用？

## 你认为怎样才是全端工程师（Full Stack developer）？

## 介绍一个你最得意的作品吧？

## 你有自己的技术博客吗，用了哪些技术？

## 对前端安全有什么看法？

## 是否了解Web注入攻击，说下原理，最常见的两种攻击（XSS 和 CSRF）了解到什么程度？

## 项目中遇到国哪些印象深刻的技术难题，具体是什么问题，怎么解决？。

## 最近在学什么东西？

## 你的优点是什么？缺点是什么？

## 如何管理前端团队?

## 最近在学什么？能谈谈你未来3，5年给自己的规划吗？

## 前端学习网站推荐
1. 极客标签：     http://www.gbtags.com/

2. 码农周刊：     http://weekly.manong.io/issues/

3. 前端周刊：     http://www.feweekly.com/issues

4. 慕课网：       http://www.imooc.com/

5. div.io：		 http://div.io

6. Hacker News： https://news.ycombinator.com/news

7. InfoQ：       http://www.infoq.com/

8. w3cplus：     http://www.w3cplus.com/

9. Stack Overflow： http://stackoverflow.com/

10.w3school：    http://www.w3school.com.cn/

11.mozilla：     https://developer.mozilla.org/zh-CN/docs/Web/JavaScript
文档推荐
jQuery 基本原理

JavaScript 秘密花园

CSS参考手册

JavaScript 标准参考教程

ECMAScript 6入门



