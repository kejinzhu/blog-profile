---
title: axios学习笔记一
copyright: true
date: 2018-09-15 20:50:33
categories: axios
tags:
    - axios
    - 请求合并以及参数配置
---

## axios请求合并以及参数配置

实现方式一：

```javascript
axios.get('A').then(resultA=>{
            return axios.get('B');
        }).then(resultB=>{
            //=>A和B都成功执行：resultB是B成功后的结果
            //=>全局result是A的结果
        })
```

实现方式二：

```javascript
//=>sendAry中存放三个ajax请求的promise实例 

let sendAry = [
    axios.get(''),
    axios.get(''),
    axios.post('')
];

// =>三个请求都完成才做一些事情,可以基于all实现

axios.all(sendAry).then(result=>{
    console.log(result);//=>是一个数组,分别存储每个请求的结果
});
```

## 一次性并发多个请求

将得到的结果分开

```javascript
//=>sendAry中存放三个ajax请求的promise实例 

let sendAry = [
    axios.get(''),
    axios.get(''),
    axios.post('')
];

// =>三个请求都完成才做一些事情,可以基于all实现

//=>匿名函数

let anonymous = axios.spread((resA,resB,resC)=>{
    //=>resA,resB,resC分别代表三次请求的结果
    //=>原理是JS中的柯理化函数
});

axios.all(sendAry).then(anonymous);
```

## 初始化一些配置项

```javascript
//=>初始化一些常用的配置项    
axios.defaults.baseURL = '';
axios.defaults.validateStatus = function validateStatus(status) {   //=>自定义成功失败规则:resolve/reject(默认规则：状态码以2开头算做成功)
    return /^(2|3|4)\d{2}$/.test(status);
};
axios.defaults.timeout = 3000;
aioxs.defaults.headers = {//=>自定义请求头
    name:'kjz'
};
axios.defaults.params = {};//=>get传参
axios.defaults.data ={};//=>post请求
// =>使用
axios.get('/list',{
    params:{
        lx:12
    }
}).then(result=>{
    //=>result.headers:服务器返回的响应头信息
})
<!-- post请求是三个参数：axios.post([url],[data],[config]) -->
axios.post('/add',{
    lx: 2,
    headers:{'xxx':'www'}//=>会放到请求主体里
},{
    headers:{'xxx':'www'},//=>配置项
    transformRequest(data){

    }
}).then(result=>{

})
```

## application/x-www-form-urlencoded:
设置在post请求中基于请求主体向服务器发送内容的格式,默认是raw，项目中常用的是urlencoded

## 设置响应拦截器

```javascript
axios.interceptors.response.use(function success(result){
    return result.data;
},function error(){

})
```

设置响应拦截器：分别在响应成功和失败的时候做一些拦截处理(在执行成功后设定的方法之前,会先执行拦截器中的方法)


## transformRequest

```javascript
axios.defaults.transformRequest = data=>{
            //=>data:就是请求主体中需要传递给服务器的内容(对象)
            let str = ``;
            for(let attr in data){
                if(data.hasOwnProperty(attr)){
                    str += `${attr} = ${data[attr]}&`
                }
            }
            return str.substring(0,str.length-1);
        }
```