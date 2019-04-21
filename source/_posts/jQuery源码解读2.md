---
title: jQuery源码解读2
date: 2018-08-14 21:28:42
tags: 
    - jQuery源码解读
    - jQuery
---

# 构造jQuery对象
 jQuery对象是一个类数组对象，含有连续的整形属性、length属性和大量的jQuery方法。jQuery对象右构造函数jQuery创建，$()是jQuery()的缩写
 <!-- more -->

## 构造函数jQuery
 调用构造函数时传入的参数不同，创建jQuery对象的逻辑也会随之不同，构造函数有7种用法；

### jQuery(selector[,context])
 这个函数接收一个包含CSS选择器的字符串，然后用这个字符串去匹配一组元素。jQuery 的核心功能都是通过这个函数实现的。jQuery中的一切都基于这个函数，或者说都是在以某种方式使用这个函数。这个函数最基本的用法就是向它传递一个表达式（通常由CSS选择器组成），然后根据这个表达式来查找所有匹配的元素。默认情况下,如果没有指定context参数，$()将在当前的 HTML document中查找 DOM 元素；如果指定了 context 参数，如一个 DOM 元素集或 jQuery 对象，那就会在这个 context 中查找。在jQuery 1.3.2以后，其返回的元素顺序等同于在context中出现的先后顺序。

### jQuery(html,[ownerDocument])
 根据提供的原始 HTML 标记字符串，动态创建由 jQuery 对象包装的 DOM 元素。同时设置一系列的属性、事件等。
 你可以传递一个手写的HTML字符串，或者由某些模板引擎或插件创建的字符串，也可以是通过 AJAX 加载过来的字符串。但是在你创建 input 元素的时会有限制。当然这个字符串可以包含斜杠(比如一个图像地址)，还有反斜杠。当你创建单个元素时，请使用闭合标签或XHTML格式。例如，创建一个 span ，可以用$("<span/>") 或 $("<span></span>") ，但不推荐 $("<span>")。在jQuery中，这个语法等同于$(document.createElement("span")) 。

### jQuery(element)、(elementArray)
 如果传入一个DOM元素或者一个DOM元素数组，则将DOM元素封装成jQuery对象并返回，那么这个DOM元素就变成了jQuery对象，可以调用jQuery上的方法。
比如：在监听函数中，或者在事件中的this,$(this)之后，这个this就由原生的对象变成jQuery对象，就可以调用jQuery上的方法，比如sideUp、slideDown...

```javascript
$("#box").click(function(){
$(this).slideUp();
})
```

### jQuery(Object)
 传入一个普通的JavaScript对象，jQuery就会把这个JavaScript对象包装成jQuery对象并返回，方便在对象上绑定事件和方法

### jQuery(callback)
 如果传入一个函数，则在document上绑定一个ready事件监听函数，ready事件的监听函数的触发要早于onload事件，ready事件的触发是在DOM结构加载完成时触发，而onload事件是在所有资源加载完成时触发。

### jQuery(jQuery Object)
如果传入的是一个jQuery对象，则重新拷贝一份，将拷贝副本返回，这个副本跟原来的jQuery对象的用法是一样的，都可以调用jQuery上的方法。他们引用的是完全相同的DOM元素

### jQuery()
 如果不传入任何东西，则返回的是一个空的jQuery对象，它的length属性是0
![构造函数的7个用法](/images/构造函数jQuery.png)
