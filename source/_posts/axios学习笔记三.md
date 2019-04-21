---
title: axios学习笔记一
copyright: true
date: 2018-09-15 20:50:33
categories: axios
tags:
    - axios
    - fetch中的基础语法
---

## fetch

fetch:不是ajax，它诞生的目的是为了ajax,它是JS 中内置的API;基于fetch可以实现客户端和服务器端的信息通信
<!-- more -->

1、fetch是2018规范中新增的API,所以浏览器的支持度不是很好(可以基于babel的最新语法解析包把其进行解析),想要兼容性好一些,需要使用"fetch ployfill"

2、使用fetch发送请求

- GET/POST等请求不能设置body
- 不管服务器返回的状态是多少,fetch都不认为是失败(哪怕是4或者5开头的状态码),都执行的是then方法,(需要我们自己进行异常操作处理)

```javascript
fetch('url',{
    method:'get',
    body:'a=2&b=3',
    headers:{
        //=>设置请求头
        'content-type':'x-www-form-urlencoded'
    }
    credentials:'include'//=>不管同源还是跨域请求都带着cookie信息
}).then(result=>{
    console.log(result);
    /**
    headers:{}包含响应头信息
    redirected:false 是否重定向
    status:状态码
    statusText:状态信息
    type:'base/cors' 状态类型
    url:''请求的地址
    __proto__:Response
    arrayBuffer()
    blob()
    json()
    text()
    ...
    基于这些方法可以快速的把从服务器中获取到的
    */
}).catch(msg=>{

})
```

get请求

```javascript
fetch('').then(result=>{
    let {status} = result;
    if(/^(4|5)\d{2}$/.test(status)){
        throw new Error('query data is error');手动抛出异常
        return ;
    }
}).then(result=>{
    console.log(result);
}).catch(msg=>{
    console.log(msg);
})
```

post请求

```javascript
fetch('',{
    methof:'post',
    body:'a=1&b=2'//=>body只支持字符串
}).then(result=>{
    let {status} = result;
    if(/^(4|5)\d{2}$/.test(status)){
        throw new Error('query data is error');手动抛出异常
        return ;
    }
}).then(result=>{
    console.log(result);
}).catch(msg=>{
    console.log(msg);
})
```