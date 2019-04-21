---
title: react学习笔记(十五)
copyright: true
date: 2018-09-08 09:21:55
categories: react
tags:
    - react 
    - redux基础
---

## REDUX
REDUX：组件实现信息共享交互的解决方案(本身是一个插件->思想),应用到任何项目当中(包含Vue项目)
REACT-REDUCT/DVA/MOBX: 为react项目提供研发的(除了MOBX其余的都是基于REDUX完成的)
VUEX：为VUE项目研发的
<!-- more -->

前提：一般在SPA单页面应用中,我们才会基于REDUX管理,因为MPA多页面应用,在页面跳转的时候,之前的作用域会释放,存储的信息(包含REDUX信息会销毁=>REDUX不是本地存储,页面刷新或者关闭,存储的信息就会消失)

## createStore:创建REDUX容器

createStore创建REDUX容器(存储公共状态信息)

![redux原理图](/images/redux原理图.png)

## REDUX流程

- 1、创建一个REDUX容器(附带了一个事件池)：createStore

```javascript
store:{
   dispatch:派发任务给reducer
   subscribe:向事件池中追加方法
   get-state:获取REDUX事件池中的状态信息
  }
```

- 2、执行createStore必须指定一个管理员reducer(reducer是一个函数),在reducer中完成状态信息的更新和管理

```javascript
//=>开始创建容器的时候,容器中没有状态信息,那么第一次执行reducer(只有dispatch才会通知reducer才会执行)，此时state是没有信息的,为了防止报错，我们给其一贯初始状态信息值即可 
function reducer(state = { 
    n: 0,
    m: 0 
}, action) {
//=>reducer默认会有一个state参数：存储的是当前redux容器中的状态信息
//=>第二个参数action:action是派发任务(dispatch)传递的行为对象:store.dispatch({type:'xxx'})type行为标识是action中必须具有的属性,这个是语法规范,只有这样reducer才能根据不同标识做不同的事情
state = JSON.parse(JSON.stringify(state));//=>进来后先进行原始状态的深度拷贝,防止state的操作直接修改原始状态信息,我们需要最后return的时候才修改

    return this.state;//=>返回的是什么,相当于把原有的状态信息修改成为什么
}
let store = createStore(reducer);
```

Vote.js文件

```javascript
import React from "react";
import PropTypes from "prop-types";
import VoteBody from "./VoteBody";
import VoteFooter from "./VoteFooter";
import VoteHeader from "./VoteHeader";
import { createStore } from "redux";


let store = createStore(function reducer(state = {
    n:0,
    m:0
}, action) {
    state = JSON.parse(JSON.stringify(state));
    if (action.type === 'A') {
        state.n++;
    }
    if(action.type === 'B'){
        state.m++;
    }
    return state;
})
store.dispatch({
    type: 'A'
})
export default class Vote extends React.Component {
    static defaultProps = {
        title: '珠峰培训教学很棒~'
    };
    static proptypes = {
        title: PropTypes.string
    }
    // =>把store放到上下文当中,这样后期哪一个后代组件需要使用store直接基于上下文获取到即可(属性传递也可以,但是操作比较麻烦)
    static childContextTypes = {
        store:PropTypes.object
    }
    getChildContext() {
        return {
            store
        }
    }
    constructor(props, context) {
        super(props, context);
    }
    render() {
        let { n, m } = this.state,
            rate = n + m === 0 ? '100%' : (n / (n + m) * 100).toFixed(2) + '%';
        let styleOBJ = {
            width: '50%',
            margin: '5% auto'
        }
        return (<section className="panel panel-default" style={styleOBJ}>
            {/* 父组件Vote把接收到的标题基于title属性传递给子组件header */}
            <VoteHeader />
            <VoteBody />
            {/* 把父组件的某一个方法当做属性传递给子组件 */}
            <VoteFooter />
        </section>)
    }
    change = flag => {
        flag ? this.n++ : this.m++;
        this.forceUpdate();//强制更新
    }
}

```

VoteHeader.js

```javascript
import React from "react";
import PropTypes from "prop-types";
export default class VoteHeader extends React.Component{
    // 需要用到哪一个上下文信息，就指定哪一个类型即可(类型和之前设定的类型一致才可以,它也是在constructor之前执行)

    static contextTypes={
        store:PropTypes.object
    }
    constructor(props, context){
        super(props,context);
    }
    render(){
        return (<header className="panel-heading">
            <h3>{this.context.tt}</h3>
        </header>)
    }
}
```

VoteBody.js组件

```javascript
import React from "react";
import PropTypes from "prop-types";
export default class VoteBody extends React.Component{
    static contextTypes ={
        store:PropTypes.object
    }
    constructor(props,context){
        super(props,context)
    }
    componentDidMount(){
        //=>第一次渲染完成之后，我们向REDUX事件池中追加一个方法(方法用来重新渲染当前组件),当redux状态改变,组件会重新渲染即可
        //=>subscribe:向事件池中追加方法,返回一个 函数,这个返回的函数执行就是把当前追加的方法从事件池移除
        let unsubscribe = this.context.store.subscribe(()=>{
            this.forceUpdate();//=>强制渲染当前组件,把更新组件的方法放事件池中
        })
        unsubscribe();//=>fn执行的时候就把事件池中的方法移除了
    }
    render(){
        let {store} = this.context,
        {n,m} = store.getState(),
        rate = m+n===0?'100%':(n/(n+m)*100).toFixed(2)+'%';
        return (<main className="panel-body">
            支持人数：{n} 人<br />
            反对人数：{m}人 <br />
            支持比率：{rate} <br />
        </main>)
    }
}
```

VoteFooter.js

```javascript
import React from "react";
import PropTypes from "prop-types";
export default  class VoteFooter extends React.Component{
    static contextProps = {
        store:PropTypes.object
    }
    constructor(props,context){
        super(props,context);
    }
    // =>this.props.handleClick:父组件传递过来的方法(方法是父组件的)
    render(){
        // 通过事件委托实现：点击的时候把父组件的方法执行,并且根据点击按钮的不同,传递不同的信息过去
        let change = this.context;
        return (<footer className="panel-footer">
            <button className="btn btn-success" onClick={()=>{
                store.dispatch({
                    type:'A'
                })
            }}>支持</button>&ensp;&ensp;
                <button className="btn btn-danger" onClick={()=>{
                    store.dispatch({
                        type:'B'
                    })
                }}>反对</button>
        </footer>)
    }
}
```