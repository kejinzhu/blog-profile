---
title: '[React]-switch和redirect'
copyright: true
date: 2018-10-04 14:20:13
categories: react
tags:
    - react
    - switch和redirect
---
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
import React from 'react';
import ReactDOM from 'react-dom';
import {HashRouter,Route,Switch,Redirect} from 'react-router-dom';
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

默认情况下,会和每一个route都做校验(哪怕之前已经有校验成功的),Switch组件可以解决这个问题,和switch case一样,只要有一种情况校验成功,就不再向后校验了

*/

ReactDOM.render(<HashRouter>
    <Switch>
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
        {/*上述都设置完成后,会在默认设置一个匹配:以上都不符合的情况下,我们认为路由地址是非法地址,我们做一些特殊处理(route不设置path是匹配所有地址规则的)*/}
        <Route render={()=>{
             return (<div>404</div>)
         }}/>
         {/*
         * 也可以基于Redirect进行重定向
         * to[string]:重定向到新的地址
         * to[object]:重定向到新的地址,只不过指定了更多的信息
         * {
             {
                 pathname:'/',
                 search:给定向的地址问号传参,(结合当前案例,真实项目中,我们有时候会根据是否存在问号参数值来统计是正常进入首页还是非正常跳转过来的,也有可能根据问号参数值做不同的事情)
                 state:给定向后的组件传递一些信息
             }
         * }
         * push:如果设置了这个属性,当前跳转的地址会加入到
         * history stack中一条记录
         * from:设定当前来源的页面地址
         * <Redirect from="/custom" to="/custom/list"/> 
         * 如果当前请求的hash地址是"custom",我们让其重定向到" * custom/list"
         */}
         {/*
         <Redirect to="/?lx=404"/>
         */}
         <Redirect to={{
             pathname:'/',
             search:'?lx=404'
         }}/>
    </Switch>
</HashRouter>,root)
```

