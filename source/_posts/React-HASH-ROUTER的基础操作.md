---
title: '[React]-HASH-ROUTER的基础操作'
copyright: true
date: 2018-10-04 11:49:08
categories: react
tags:
    - react
    - hash-router的基础操作
---
## react-router-dom实现SPA单页面应用

> yarn add react-rouert-dom

### 使用react路由实现SPA
1、安装路由：yarn add react-router-dom
    3及以前版本称为react-router
    4及最新版本称为react-router-dom
2、学习react路由：
    http://reacttraining.cn/web/api/
3、BrowserRouter VS HashRouter    
    它是两种常用的路由实现思想,BrowserRouter是浏览器路由,HashRouter是哈希路由
[BrowserRouter]:

它是基于H5中的history API(pushState,replaceState,popState)来保持UI和URL的同步。真实项目中应用的不多,history API并不是很稳定，一般只有当前项目是基于服务器渲染的,我们才会使用浏览器路由
http://www.demo.com/
http://www.demo.com/personal/
http://www.demo.com/personal/login/

[HashRouter]:
真实项目中(前后端分离的项目：客户端渲染,我们经常使用的是哈希路由来完成的,它依据相同的页面地址,不同的哈希值,来规划当前页面中的哪一个组件呈现渲染,它基于原生JS构造了一套类似于history API的机制,每一次路由切换都是基于history stack完成的!)

http://www.demo.com/#/
http://www.demo.com/#/personal
http://www.demo.com/#/personal/login

A页面

```javascript
import React from 'react';
export default class A extends React.Component{
    constructor(props){
        super(props);
    }
    render(){
        return (<section>我是A组件</section>)
    }
}
```

B页面

```javascript
import React from 'react';
export default class B extends React.Component{
    constructor(props){
        super(props);
    }
    render(){
        return (<section>我是B组件</section>)
    }
}
```

C页面

```javascript
import React from 'react';
export default class C extends React.Component{
    constructor(props){
        super(props);
    }
    render(){
        return (<section>我是C组件</section>)
    }
}
```

index.js入口文件

```javascript
import React,{Fragment} from 'react';
import ReactDOM from 'react-dom';
import {HashRouter,Route} from 'react-router-dom'
import A from './component/A';
import B from './component/B';
import C from './component/C';

/*
*  HashRouter:
*  1、当前项目一旦使用Hash-Router,则默认在页面的地址后面加上'#/',也就是Hash默认值是一个斜杆,我们一般让其显示首页组件信息内容
*  2、Hash-Router只能出现一个子元素
*  3、Hash-Router机制中,我们需要根据哈希地址不同,展示不同的组件内容,此时需要使用Route
*  4、Route
*       path:匹配哈希后面的值(地址),但是默认不是严格匹配,当前页面哈希地址只要包含完整的它(内容时不变的)都能被匹配上
        例如：path="/":和它匹配的地址只要有斜杆(都能和它匹配)
        path="/user":'#user/login'也可以匹配,但是"#/user2"这个无法匹配
        component：一旦哈希值和当前route的path相同了,则渲染component指定的组件    
        exact：让path的匹配严谨和严格一些(只有url哈希值和path设定的值相等才可以匹配到)
        path="/":"#/"匹配,但是"#/user"就不再匹配了
    strict    
    render：当页面的哈希地址 和path匹配，会把render规划的方法执行，在方法中一般做"权限校验"(渲染组件之前验证是否存在权限,不存在做一些特殊处理)

    ---------------------

默认情况下,会和每一个route都做校验(哪怕之前已经有校验成功的)

*/

ReactDOM.render(<HashRouter>
    <Fragment>
        <Route exact path='/' component={A}/>
        <Route path='/user' component={B}/>
        <Route path="/pay" render={()=>{
            //=>一般在render中处理的是权限校验
            let flag = localStorage.getItem('flag');
            if(flag&&flag === 'safe' ){
                return <C/>
            }
            return '当前环境不安全,不利于支付';
        }}/>
    </Fragment>
</HashRouter>,root)
```
