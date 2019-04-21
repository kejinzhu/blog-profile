---
title: react学习笔记(十一)
copyright: true
date: 2018-09-07 11:39:42
categories: react
tags:
    - react学习笔记
    - react组件的生命周期
---
生命周期函数(钩子函数)
描述一个组件或者程序从创建到销毁的过程，我们可以在过程中间基于钩子函数完成一些自己的操作(例如：在第一次渲染完成做什么,或者在第二次即将重新渲染之前做什么等...)
<!-- more -->

## [基本流程]

- constructor 创建一个组件
- componentWillMount 第一个渲染之前
- render 第一次渲染
- componentDidMount 第一次渲染之后

## [修改流程]

当组件的状态数据发生改变(setState)或者传递给组件的属性(重新调用组件，传递不同的属性)都会引发render重新执行渲染(渲染也是差异渲染(哪个跟之前不一样，才渲染))

- shouldComponentUpdate 是否允许组件重新渲染(允许则执行后面函数,不允许直接结束即可)
- componentWillUpdate 重新渲染之前
- render 第二次及以后重新渲染
- componentDidUpdate 重新渲染之后
- componentWillReceiveProps：父组件把传递给子组件的属性发生改变后触发的钩子函数

## [卸载]
原有渲染的内容是不消失的，只不过以后不能基于数据改变视图了

- ComponentWillUnmount：卸载组件之前(一般不用)

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import 'bootstrap/dist/css/bootstrap.css';
import PropTypes from "prop-types";

function queryData() {
    return new Promise((resolve,reject)=>{
        setTimeout(() => {
            resolve()
        }, 3000);
    })
}
class A extends React.Component{
    static defaultProps = {};//这是第一个执行的，执行完成之后(给属性设置默认值)才向下执行
    constructor(){
        super();
        console.log('1=construtor');
        this.state({
            n:1
        });
    }
    componentWillMount(){
        // 第一次渲染执行
        console.log('2=WillMount:第一次渲染之前',this.refs.hh);//undefined
        // 在WillMount中，如果直接的set-state修改数据，会把状态信息改变后，然后人的人和Didmount；但是如果set-state是放到一个异步操作中完成(例如：定时器或者从服务器获取数据)，也是先执行render和Did，然后再执行这个异步操作修改状态，紧接着走修改的流程(这样和放到Did_Mount中没有区别),所以我们一般把数据请求放到Did中处理

        // 所以真实项目中的数据绑定，一般第一次组建渲染，我们都是绑定的默认数据，第二次才是绑定的从服务器获取的数据(有些需求我们需要根据是否存在判断显示隐藏符)
        this.setState({
            n:3
        })
    }
    async willMount(){
        console.log('willMount');
        let result  = await queryData();
        this.setState({
            n:result
        })
    }
    componentDidMount(){
        console.log('4=DidMount:第一次渲染完成',this.ref.hh);//div
        /**
         * 真实项目中这个阶段，一般做如下处理：
         * 1.控制状态信息的操作
         * 2.从服务器获取数据，然后修改状态信息，完成数据绑定
         */
        // setInterval(()=>{
        //     this.setState({
        //         n:2
        //     });
        // },5000)
    }
    shouldComponentUpdate(nextProps,nextState){
        // 是否允许更新，在这个钩子函数中，我们获取到的state不是最新修改的，而是上一次的state的值
        // 例如：第一次加载完成，5000ms之后。我们基于set-state把n修改为2，但是此处获取的还是1
        // 但是这个方法有两个参数：
        // 1.nextProps:最新修改的属性
        // 2.nextState:最新修改的状态信息
        console.log('5=是否允许更新,返回true就是允许，返回false就是不允许');
        
    }
    componentWillUpdate(){
        //组件更新之前
        // 这里获取的状态也是更新之前的
        console.log('6=组件更新之前');
        
        
    }
    componentDidUpdate(nextProps,nextState){
        // 这里获取的状态是更新之后的(和should相同也有两个参数存储最新的ii纳西)
        console.log('7=组件更新之后');
        
    }
    render(){

        console.log('3=render');
        
        return <div ref='hh'>
            {this.state.n}
        </div>
    }
}
ReactDOM.render(<A/>,root);
```