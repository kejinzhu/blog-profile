---
title: xss学习笔记一
copyright: true
date: 2018-09-17 10:06:16
categories: xss
tags:
    - xss
    - 学习笔记一
---

## 同源策略

思考题：
与https://segmentfault.com/index.html

- 1)https://segmentfault.com:80/lives
- 2)https://www.segmentfault.com/index.php
- 3)http://segmentfault.com/test.html
- 4)https://segmentfault.com:443/lives
- 5)data:text/html;,<html><body></body></html>

## xss的危害

2011.6.28,新浪微博受到xss蠕虫攻击
20:14,开始有大量带V的认证用户中招转发蠕虫
20:30,2kt.cn中的病毒页面无法访问
20:32,新浪微博中hellosamy中户无法访问
21:02,新浪漏洞修补完毕

2011.3.26日起,GitHub受到ddos攻击
第一轮,百度统计的JS文件被劫持篡改,每2秒向GitHub上的两个页面发出请求,被Git胡U币的弹窗警告拦住
第二轮,跨网域<img>攻击,被GitHub
第三轮,DDOs攻击GitHub Pages


## xss的成因

1、网页劫持,dns劫持,跳到假网站/页面/脚本被篡改,插入恶意代码
2、缓存投毒访问A站点时,加载了B站点的通用脚本,缓存时间非常长。再访问B站点时中招
3、文件投毒,在非官方站点下载了某个库;官方下载地址被攻击;迅雷网络加速缓存了错误的文件;引用了不可信的第三方dns上的资源
4、客户端投毒,被安装了恶意插件(尤其是chrome插件)
5、自身安全漏洞造成猥琐绕过,产生反射型和存储型xss

## 解决方案

1、网页劫持,dns劫持,跳到假网站/页面/脚本被篡改,插入恶意代码https,工信部投诉
2、缓存投毒,访问A站点时,加载了B站点的通用脚本,缓存时间非常长。再访问B站点时中招 (网站:https  个人:连公共热点全程npn)
3、文件投毒,在非官方站点下载了某个库;官方下载地址被攻击;迅雷网络加速缓存了错误的文件;引用了不可信的第三方dns上的资源  (去官网下载  只使用可靠资源)
4、客户端投毒,被安装了恶意插件(尤其是chrome插件) (谨慎筛选插件  异站异密  二次验证)
5、自身安全漏洞造成猥琐绕过,产生反射型和存储型xss

## 编码

## HTML编码
十进制：&#60;&#62;
十六进制：&#x3c;&#x3e;
别名：&lt;&gt;&copy;&newLine;&color

## JavaScript编码

八进制:\141\154\145\162\50\51
十六进制:

## url编码
[encodeURI/decodeURI]
[encodeURIComponent/decodeURIComponent]

## base64编码
<a href=data:text/html;base64,PGLtZyB></a>

## 成因

1、提前闭合标签
2、注释后续代码

<img src='https://segmentfault.com/favicon.ico' alt=''/>

1、此处可以插入任意空格、回车、/
2、可以是http https ftp mailto data javascript vbscript //...各种协议的url
3、可以是双引号、单引号、也可以是没有引号，在IE中可以是反引号
4、任意的JS，能直接访问document作用域，字符可以被HTML转译

## Payload(有效荷载)

1、<input onfocus = write(1) autofocus>
2、<img src onerror=alert(1)>
3、<svg onload=alert(1)>
4、<script>alert(1)</script>
5、<a href='javascript:alert(1)'>clickme</a>









