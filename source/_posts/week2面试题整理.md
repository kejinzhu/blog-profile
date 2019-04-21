---
title: week2面试题
copyright: true
date: 2018-09-03 15:11:45
categories: 面试题
tags:
    - 面试题
    - 第二周面试题整理
---
## 题目：请实现方法 parse ，作用如下：

```javascript
var object = {
    b: { c: 4 },
    d: [{ e: 5 }, { e: 6 }]
};
console.log(parse(object, 'b.c') == 4) //true 
console.log(parse(object, 'd[0].e') == 5) //true
console.log(parse(object, 'd.0.e') == 5) //true
console.log(parse(object, 'd[1].e') == 6) //true
console.log(parse(object, 'd.1.e') == 6) //true
console.log(parse(object, 'f') == 'undefined') //true
function parse(obj, str) {
    var arr = str.split(""); //将字符串分割，放进数组中
    arr = arr.map(function(item, index) {
            if (!isNaN(item)) {
                //判断是否是数字
                if (arr[index - 1] != '[' && arr[index + 1] != ']') {
                    //如果前后没有,则返回[item]
                    return "[" + item + "]";
                } else {
                    //如果不是上述情况，则直接返回该项
                    return item;
                }
            } else {
                //如果不是数字的情况下,.后面是数字,则需要去掉前面的.
                if (item === "." && !isNaN(arr[index + 1])) {
                    return "";
                }
                return item;
            }
        })
        //将数组join方法变成字符串赋值给str
    str = arr.join("");
    //将结果返回
    return eval('obj.' + str) || 'undefined';
}
```
<!-- more -->
题解：

- 1、首先，我们拿到题的时候，应该分析题目的目的。首先是一个object对象，然后通过parse传参的形式，将对象object和字符串作为参数传进去。返回值是获取object对象属性值，属性值存在的时候返回属性值，不存在的时候返回undefined。
- 2、获取属性值有两种方式：object[属性名]或者object.属性名，但是属性名是数字的时候只能采用第一种方式。所以我们要对这种情况进行讨论。
- 3、下面我们要讨论的就是数字这种情况的，分析数字前后有没有[]，如果有，那么我们不需要做什么，直接返回，如果数字前后没有[],那么我们是需要手动给数字前后加上[],并且把前面的'.'去掉。
- 4、分析好上述情况之后，我们代码实现：首先传入obj,str
 - 4.1、我们将str.split("")，放入数组中，然后采用数组的map方法进行遍历，分两种情况，是数字，和不是数字
 - 4.2、是数字中，又分为数字前后有没有[],没有就添加上再返回，有就直接返回
 - 4.3、不是数字的话，如果是'.',则需要判断'.'后面是否是数字，如果是数字，就返回空字符串
 - 4.4、将map映射后的数组重新赋值给arr，最后用arr.join("")拼接之后转字符串再赋值给str
 - 4.5、最后我们需要运算之后，将属性值返回来，字符串的运算，我们采用eval来运算。

## 题目：请写出如下输出值,并写出把注释掉的代码取消注释的值，并解释为什么？

```javascript
this.a = 20; //给window新增一个属性a,并赋给属性值20
var test = {
    a: 40,
    init: () => {
        console.log(this.a); //箭头函数没有this，会向上级作用域找，上级作用域是window，所以这里打印的this.a其实是window.a，打出属性值20
        function go() {
            //this.a = 60;
            console.log(this.a); //this指向实例,因为是new执行的,原型上有a=50，打印50。如果解开注释的话，自己私有属性有，就找自己私有属性，打印60
        }
        go.prototype.a = 50;
        return go;
    }
};
//var p = test.init();
//p();//这里执行的话，go中的this指向window，
new(test.init())();
```

我们从上到下一行一行分析：

- 1、this.a当前作用域是window，所以执行这句话的时候相当于给window新增了键值对a:20
- 2、再到test是一个对象,有一个init方法，是一个箭头函数，箭头函数中的this会指向window，然后箭头函数中的返回值是一个go函数
- 3、我们先不看注释掉的代码，看函数怎么执行的，先是new(test.init())()，是new test.init()也就是说是init的返回值new，那么这个时候go就是一个构造函数，init执行的时候打印this.a是20,再new go()的时候，go中的this指向它的实例，this自己没有a属性，找到原型上的a属性，打印50
- 4、因此，在不看注释的代码，依次打印的是：20、50
- 5、我们再解开注释代码，用变量p接收test.init()的返回值，也就是把go的空间地址赋值给p了，p()执行的时候，这个时候go中的this会指向window,在go中又修改了this.a的值为60；
- 6、new(test.init())()执行这句的时候，跟上面第三步的分析一样，只不过这个时候给this新增了私有属性a，就直接打印60
- 7、因此，在解开注释之后，打印的顺序依次是：20、60、60、60

## 写出下面程序的输出结果

```javascript
var arr1 = "john".split('');//arr1=['j','o','h','n']
var arr2 = arr1.reverse();//arr2=arr1=['n','h','o','j']
var arr3 = "jones".split('');//arr3=['j','o','n',e','s']
arr2.push(arr3);//arr1=arr2=['n','h','o','j',['j','o','n',e','s']]
console.log("array 1: length=" + arr1.length + " last=" + arr1.slice(-1));
console.log("array 2: length=" + arr2.length + " last=" + arr2.slice(-1));
```

题解：

- 1、arr1 = "john".split('');分割之后，得到数组：arr1=['j','o','h','n']
- 2、arr2 = arr1.reverse();由于reverse会改变原数组，将arr1.reverse的返回值给arr2，所以arr2与arr1指向同一个地址；得到：arr2=arr1=['n','h','o','j']
- 3、arr3 = "jones".split('');分割之后得到：arr3=['j','o','n',e','s']
- 4、arr2.push(arr3);将arr3直接push在末尾，得到：arr1=arr2=['n','h','o','j',['j','o','n',e','s']]
- 5、因此打印arr1和arr2的长度是5，slice(-1)截取的时候，是索引为5+(-1)的值['j','o','n',e','s']，转字符串就是'j','o','n',e','s'

## 写出下列程序的输出结果

```javascript
function f1(n) {
    n = n || 2;
    return function (x) {
        return (x * n);
    }
}
var f2 = f1();//function(x){return (x*2)}
var f3 = f1(3);//function(x){return (x*3)}
console.log(f2(3));//6
console.log(f3(3));//9
console.log(f3(f2(3)));//18
```

## 什么是跨域？常用的跨域方式有哪些？

由于浏览器存在同源策略，请求的 Url 地址的协议、主机名、端口号必须完全相同，否则会产生跨域，

同源策略的限制下 cookie 、loclstorage、dom、ajax、IndexDB 等都不允许跨域、form 表单不受同源策略限制

对跨域的理解有一个误区，跨域不是请求没有发送出去或者服务器接收到请求而没有响应，正确的情况是请求发出，服务器响应，由于响应和请求来自不同的域被浏览器拦截了。

跨域的方式有以下几种：
- 1、jsonp：通过 script 的 src 属性发送请求，传参必须含有 callback 回调的名称，服务器返回函数的调用，接收到响应直接执行；
- 2、cors：通过服务端设置 Access-Control-Allow-Origin，通常在后端通过白名单设置权限允许固定的域来访问
- 3、window.postMessage：H5 API，消息接收方通过 message 事件接收，事件对象 data 属性代表接收的消息，可以通过事件对象的 source 属性 通过 source.postMessage(“message”, e.origin) 进行回复
- 4、可以使用 window.name 和 location.hash 在不同域的 iframe 页面之间进行消息传递
- 5、document.domain：如果域名之间是一二级域名的关系，可以将页面的 document.domain 设置为一级域名的后半部分，如：baidu.com 实现跨域
- 6、可以使用 websocket 进行跨域，websocek 的协议为 ws:// 或 wss:// 是实时通信，不存在跨域问题。
- 7、服务器与服务器之间通信不受浏览器同源策略限制，因此不存在跨域问题，可以使用 nginx 或 node 的 http-proxy-middleware 中间件实现作为代理服务器帮助浏览器对请求进行转发，完成与不同域的服务器之间的通信，webpack-dev-server 就是通过 http-proxy-middleware 实现的跨域

详情请看：https://www.pandashen.com/2018/06/11/20180611010638/

## 性能优化
详情见我的博客地址：https://kejinzhu.cn/2018/08/15/web%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91%E5%B8%B8%E7%94%A8%E7%9A%84%E4%BC%98%E5%8C%96%E6%8A%80%E5%B7%A7%E6%B1%87%E6%80%BB/#more
