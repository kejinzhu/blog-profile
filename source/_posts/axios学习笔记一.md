---
title: axios学习笔记一
copyright: true
date: 2018-09-15 20:50:33
categories: axios
tags:
    - axios
    - 基础知识
---
## axios基础语法

axios:它是一个类库,基于promise管理的ajax库

1、提供了对应请求方式的方法(axios.get/post/delete/head/put/options...)
axios.get()向服务器发送一个请求，基于的是get方式
2、支持的参数配置

```javascript
axios.get([url],[options]);
axios.get('',{
    params:{//=>get请求中，会把params中的键值对拼接成urlencode格式的字符串,然后以问号传参的方式,传递给服务器,类似于JQ-ajax中的data,或者自己基于url后面拼接也可以,不用params
        name:'',
        age:9
    }
})

axios.post('',{
    //配置项中传递的内容都相当于基于请求主体专递给服务器,但是传递给服务器的内容格式是raw(json格式的字符串),不是X-WWW-FROM-URLENCODED
    name:'',
    age:9
})
```

3、基于get/post发送请求,返回的结果都是promise实例

- data=>从服务器获取的响应主体内容
- headers:从服务器获取的响应头信息
- request:ajax实例
- status:状态码
- statusText:状态码的描述
- config:基于axios发送请求的时候做的配置项