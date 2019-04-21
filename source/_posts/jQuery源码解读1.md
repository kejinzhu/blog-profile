---
title: jQuery源码解读1
date: 2018-08-14 13:50:14
categories: 
          - jQuery
          - 源码
tags: 
    - jQuery源码解读
    - jQuery@1.11.3
copyright: true    
---

# jQuery源码解读1
一万多行的jQuery源码着实让我恐惧，但是jQuery是一个优秀的JavaScript类库，里面封装的方法，jQuery源码里面把函数的三种身份变现的淋漓尽致、原型链、各种设计思想、让人不禁感叹编程之美。抱着提高编程能力和编程思想的目的，我把源码的注释删掉之后又鼓起勇气重新来看。边看边做下笔记。
先对jQuery做一下小的介绍：
<!-- more -->
首先，jQuery是一个优秀的JavaScript类库：
它提供了一些常用的方法，我们在开发项目的时候，随时可以引入这些类库，实现我们的业务需求。常用类库：jQuery(常用于PC端)，zepto(常用于移动端)。
相比于其他的类似UI组件、插件、框架有一定的不同点。
 **UI组件：**UI组件：通过html css js组成，我们使用的时候，直接引入就可以了，常用的UI组件：bootstrap antDesign
 **插件：**带有一定的业务逻辑。比如说选项卡插件，轮播图插件；我们在项目开发的时候，重复的业务可以用插件来替代；比如说常用的插件：swiper iscroll Echarts;
 **框架：**它具有一定的编程思想，我们开发的时候需要按照它提供的编程思想进行开发，它提供了相对应的ui组件和功能插件，常用的框架有：React Vue Augular Backbone

```javascript
(function( global, factory ) {
    if ( typeof module === "object" && typeof module.exports === "object" ) {
        module.exports = global.document ?
            factory( global, true ) :
            function( w ) {
                if ( !w.document ) {
                    throw new Error( "jQuery requires a window with a document" );
                }
                return factory( w );
            };
    } else {
        factory( global );
    }
}(typeof window !== "undefined" ? window : this, function( window, noGlobal ) {
....
if ( typeof noGlobal === strundefined ) {
        window.jQuery = window.$ = jQuery;
    }
    return jQuery;
}));
```

## 自调用匿名函数”
 首先，源码一开始采用了闭包的方法，将代码放在了一个自执行函数中，这种方法称之为“自调用匿名函数”，当浏览器加载完成jQuery文件之后，就会立即执行该函数，初始化各个模块。

### 为什么需要自调用匿名函数呢？
 通过自调用匿名函数创建了一个特殊的作用域，保护里面的私有变量不受外界干扰，避免了全局变量污染的问题。由于jQuery会被应用在成千上万的代码中，为了防止命令以及变量的冲突，自调用函数的设计就防止了与外界变量的冲突。
那么，外界需要用到jQuery对象的话，应该怎么办呢？jQuery提供了一个出口，将jQuery对象暴露在全局对象window下,window.jQuery = window.$ = jQuery;也就是说，在外界只要执行jQuery或者$就可以使用jQuery对象封装的方法了。

### 为什么自调用匿名函数需要传入window/global？和工厂函数？
 因为在浏览器端的全局对象是window，如果程序跑在服务端的node环境下，全局变量是global，将全局变量作为参数传递进去时，jQuery对象就可以更快的访问到全局对象，另外，全局对象作为参数传递进去的时候，可以在代码压缩的时候进行优化;

### 自调用匿名函数前后不能省略分号
 如果没有给自调用匿名函数的末尾加上分号，那么下一个匿名函数的第一个括
 号会被当做函数来执行。比如

```javascript
(function(){})()
(function(){})()
//undefined is not a function
```
