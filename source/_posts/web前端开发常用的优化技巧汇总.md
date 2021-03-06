---
title: web前端开发常用的优化技巧汇总
date: 2018-08-15 16:29:16
toc: true
tags:
    - web前端
    - 性能优化
---
对于一个前端工程师，不仅要写好代码，还要考虑到性能方面的优化问题，那么我们可以从哪些方面进行性能的优化？
首先，我们先来看看“雅虎军规”的35条：
<!-- more -->
# "雅虎军规"35条

- 尽量减少 HTTP 请求个数——须权衡
- 使用 CDN（内容分发网络）
- 为文件头指定 Expires 或 Cache-Control ，使内容具有缓存性。
- 避免空的 src 和 href
- 使用 gzip 压缩内容
- 把 CSS 放到顶部
- 把 JS 放到底部
- 避免使用 CSS 表达式
- 将 CSS 和 JS 放到外部文件中
- 减少 DNS 查找次数
- 精简 CSS 和 JS
- 避免跳转
- 剔除重复的 JS 和 CSS
- 配置 ETags
- 使 AJAX 可缓存
- 尽早刷新输出缓冲
- 使用 GET 来完成 AJAX 请求
- 延迟加载
- 预加载
- 减少 DOM 元素个数
- 根据域名划分页面内容
- 尽量减少 iframe 的个数
- 避免 404
- 减少 Cookie 的大小
- 使用无 cookie 的域
- 减少 DOM 访问
- 开发智能事件处理程序
- 用 代替 @import
- 避免使用滤镜
- 优化图像
- 优化 CSS Spirite
- 不要在 HTML 中缩放图像——须权衡
- favicon.ico要小而且可缓存
- 保持单个内容小于25K
- 打包组件成复合文本

在性能优化方面，我们可以从三个方面来考虑
# 减少http请求的次数或者减少请求数据的大小

页面中每发送一次`HTTP`请求，都需要完成请求+响应这个完整的HTTP事务,会消耗一些时间，也可能会导致`HTTP`连接通道阻塞，为了提高页面加载速度和运行的性能，我们应该减少`HTTP`的请求次数和减少请求内容的大小(请求的内容越大，消耗的时间越长)
## 1.采用CSS雪碧图技术(CSS Sprit/CSS图片精灵)技术，把一些小图合并在一张大图上,使用的时候通过背景图片定位,定位到具体的某一张小图上

```css
.pubBg{
	background:url('../img/sprit.png') no-repeat;
	background-size:x y /*=>和原图大小保持一致的;*/
}
.box{
	background-position:x y;/*通过背景定位,定位到具体的位置，展示不同的图片即可*/
}
....
<div class = 'pubBg box'></div>
```

## 2、真实项目中，我们最好把CSS或者JS文件进行合并压缩；尤其是在移动端开发的时候，如果CSS或者JS文件内容不是很多，我们可以采取内嵌式，告别外链式，依次来减少HTTP请求次数，加快页面加载速度；

- 1)CSS合并成一个，JS最好也合并成一个
- 2)首先通过一些工具(例如：webpack)把合并后的CSS或者JS压缩成xxx.min.js，减少文件大小
- 3)服务器端开启资源文件的GZIP压缩
...

通过一些自动化工具完成CSS以及JS的合并压缩，或者再完成LESS转CSS，ES6转ES5等操作，我们把这种自动化构建模式，称之为前端`"工程化"`开发

## 3、`采用图片懒加载技术`，在页面开始加载的时候，不请求真实的图片地
址，而是用默认图占位，当页面加载完成后，再根据相关的条件依次记载真实图片(核心目是：减少页面首次加载HTTP请求的次数)
真实项目中，我们开始图片都不加载，页面首次加载完成，先把第一屏中可以看见的图片进行加载，随着页面滚动，再把下面区域中能够呈现出来的图片进行加载

根据图片懒加载技术，我们还可以扩充出,**`数据的懒加载`**

- 1)开始加载页面的时候，我们只把首屏或者前两屏的数据从服务器端进行请求(有些网站首屏数据是后端渲染好，整体返回给客户端呈现的)
- 2)当页面下拉，滚动到哪个区域，再把这个区域需要的数据进行请求(请求回来做数据绑定以及图片延迟加载)
- 3)分页展示技术采用的也是数据的懒加载思想实现的：如果我们请求获取的数据是很多的数据，我们最好分批请求，开始只请求第一页的数据，当用户点击第二页(微博是下拉到一定的距离之后，再请求第二页数据...)的时候再请求第二页的数据...
- 4、对于不经常更新的数据，最好使用浏览器的`304`缓存做处理(主要由服务器端处理)

 **例如：**

第一次请求CSS和JS下来，浏览器会把请求的内容缓存在了，如果做了`304`处理，用户再次请求CSS和JS，直接从缓存中读取，不需要再去服务器获取了(减少了HTTP请求次数)
当用户强制刷新页面(CTRL+F5)或者当前缓存的CSS或JS发生了变动，都会重新的服务器端拉取
 ...
 
 对于客户端来讲，我们还可以基于localStorage来做一些本地存储，例如：第一次请求的数据或者不经常更新的CSS和JS，我们都可以把内容存储在本地，下一次页面加载，我们从本地中获取即可，我们设定一定的期限或者一些标识，可以控制在某个阶段重新从服务器获取

## 5、使用字体图标代替一些页面中的位图(图片)，这样不仅做适配的时候方便，而且更加轻量级，而且减少了HTTP请求次数(类似于图片雪碧图)
## 6、如果当前页面中出现了AUDIO或者VIDEO标签，我们最好设置它们的`preload=none;页面加载的时候，音视频资源不进行加载，播放的时候再开始加载`(减少页面首次加载HTTP请求的次数)
 `preload=auto;`页面首次加载的时候就把音视频资源进行加载了
 `preload=metadata;`页面首次加载的时候只把音视频资源的头部信息进行加载 

## 7、在客户端和服务器端进行数据通信的时候，我们尽量采用JSON格式进行数据传输

**[优势]**
1）JSON格式的数据，能够清晰明了的展示出数据结构，而且也方便我们获取和操作
2）相对于很早以前的XML格式传输，JSON格式的数据更加轻量级
3）客户端和服务器端都支持JSON格式数据的处理，处理起来非常的方便
真实项目中，并不是所有的数据都要基于JSON，我们尽可能这样做，但是对于某些特殊需求(例如：文件流的传输或者文档流的传输)，使用JSON就不合适了

## 8、采用CDN加速
 **CDN：** 分布式(地域分布式)[很烧钱]

**本质上：`真正的性能优化是由硬件做依托的`**

# 关于编写代码时候的一些优化技巧

除了减少HTTP请求次数和减少请求数据的大小可以优化性能，我们在编写代码的时候，也可以进行一些优化，让页面的性能有所提升(有些不好的代码编写习惯，会导致页面性能消耗太大，例如：内存泄露)

## 1、在编写JS代码的时候，尽量减少对DOM的操作(VUE和react框架在这方面处理的非常不错)

在JS中操作DOM是一个非常消耗性能的事情，但是我们又不可避免的操作DOM，我们只能尽量减少对它的操作

**[操作DOM的弊端]**

- 1)DOM存在映射机制(JS中的DOM元素对象和页面中的DOM结构是存在映射机制的，一改则都改)，这种映射机制，是浏览器按照W3C标准完成对JS语言的构建和DOM的构建(`其实就是构建了一个监听机制`)，操作DOM是同时要修改两个地方，相对于一些其他的JS编程来说是消耗性能的
- 2)页面中的DOM结构改变或者样式改变，会触发浏览器的回流(浏览器会把DOM结构重新进行计算，这个操作很耗性能)和重绘(`把一个元素的样式重新渲染`)

## 2、编写代码的时候，更多的使用异步编程

 **同步编程会导致：**上面东西完不成，下面任务也做不了，我们开发的时候，可以把某一区域模块都设置为异步编程，这样只要模块之间没有必然的先后顺序，都可以独立进行加载，不会受到上面模块的堵塞影响(用的不多)
 
 尤其是`ajax`数据请求，我们一般都要使用异步编程，最好是基于`promise`设计模式进行管理(项目中经常使用`fetch、VUE axios`等插件来进行`ajax`请求处理，因为这些插件中就是基于`promise`设计模式对`ajax`进行的封装处理)

## 3、在真实项目中，我们尽可能避免一次性循环过多数据(因为循环操作是同步编程)，尤其是要避免while导致的死循环操作
## 4、CSS选择器优化

 1）尽量减少对标签选择器的使用
 2）尽可能少使用ID选择器，多使用样式类选择器(通用性强)
 3）减少使用选择器时候前面的前缀，例如：`headerBox .nav .left a{}`(选择器是从右向左查找的)、在命名的时候做优化。

## 5、避免使用CSS表达式

```css
/*CSS表达式*/
.box{
	background-color:expression((new Date()).getHours()%2?'red':'blue');
}
```

## 6、减少页面中的冗余代码，尽可能提高方法的重复使用率："低耦合高内聚"
## 7、最好CSS挡在head中，而JS放在body尾部，让页面加载的时候，先加载CSS，再加载JS(先呈现页面，再给用户提供操作)
## 8、JS中避免使用eval

 1）性能消耗大
 2）代码压缩后，容易出现代码执行错乱问题

## 9、JS中尽量减少闭包的使用

 1）闭包会形成一个不销毁的栈内存，过多的栈内存累积会影响页面的性能
 2）还会容易导致内存的泄露
 
 闭包也有自己的优势：保存和保护，我们只能尽量减少，但是无可避免

## 10、在做DOM事件绑定的时候，尽量避免一个个的事件绑定，而是采用性能更高的事件委托来实现

**事件委托(事件代理)**

把事件绑定给外层，当里面的后代元素相关行为被触发，外层容器绑定的方法也会被触发执行(冒泡传播机制导致)，通过事件源是谁，我们做不同的操作即可

## 11、尽量使用CSS3动画代替JS动画，因为CSS3的动画或者变形都开启了硬件加速，性能比JS动画好

## 12、编写JS代码的时候尽可能使用设计模式来构建体系，方便后期的维护，也提高了扩充性等

## 13、CSS中减少对滤镜的使用，页面中也减少对flash的使用

# 关于页面SEO优化技巧
## 1、页面中杜绝出现死链接(404页面)，而且对于用户输入一个错误页面，我们要引导到404提示页面中(服务器处理的)
## 2、避免浏览器中异常错误的抛出

 尽可能避免代码出错
 使用try catch做异常信息的捕获
 ...

## 3、增加页面关键词优化
