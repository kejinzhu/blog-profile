---
title: react学习笔记(六)
copyright: true
date: 2018-09-07 11:39:42
categories: react
tags:
    - react学习笔记
    - 基于类创建React组件
---

## 基于继承component类来创建组件

基于createElement把JSX转换为一个对象，当render渲染这个对象的时候，遇到type是一个函数或者类，不是直接创建元素，而是先把方法执行;

<!-- more -->

- 如果就是函数式声明组件，就把它当做普通方法执行(方法中的this是undefined),把函数返回的JSX元素(也是解析后的对象)进行渲染
- 如果是类声明式的组件，会把当前类new它执行，创建类的一个实例(当前本次调取的组件就是它的实例),执行constructor之后，会执行this.render(),把render返回的JSX拿过来渲染，所以"类"声明式组件,必须有一个render方法,方法需要返回一个JSX元素

但是不管哪一种方式，最后都会把解析出来的props属性对象作为实参传递给对应的函数或者类

### 总结

创建组件有两种方式"函数式"、"创建类式"

#### 函数式

- 1、操作非常简单
- 2、能实现的功能也很简单，只是简单的调取和返回JSX而已
- 3、

#### 创建类式

- 1、操作相对复杂一些，但是也可以实现更为复杂的业务功能
- 2、能够使用生命周期函数操作业务
- 3、函数式可以理解为静态组件(组件中的内容调取的时候就已经固定了,很难在修改),而类这种方式交由基于组件内部的状态更新渲染的内容

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
function Sum() {
    console.log(this);//undefined
    return <div>
        函数声明~
    </div>
}
class Dialog extends React.Component{
    constructor(props){//props,context,updater

        super(props);//=>ES6中extends继承，一旦使用了constructor,第一行位置必须设置super执行，相当于React.Component.call(this),也就是call继承,把父类私有的属性继承过来

        // =>如果只写super()：虽然创建实例的时候把属性传递进来了，但是并没有传递父组件,也就是没有吧属性挂载到实例上,使用this.props获取的结果是undefined
        // =>如果super(props)：在继承父类私有属性的时候,就把传递的属性挂载到了子类的实例上,constructor中就可以使用this.props了

        // =>props:当render渲染并且把当前类执行创建实例的时候，会把之前JSX解析出来的props对象中的信息(可能有children)传递给参数props "调取组件传递的属性"

        /**
         * this.props:属性集合
         * this.refs:ref集合(非受控组件中用到)
         * this.context:执行上下文
         * this.updater:
         */

        console.log(props);
        console.log(this.props);
        //=>及时在constructor中不设置形参props接收属性，执行super的时候也不传这个属性,除了constructor中不能直接使用this.props,其他声明周期函数中都可以使用(也就是执行完成constructor，react已经帮我们把传递的属性接收,并且挂载到实例上了)
    }
    // componentWillMount(){
    //     //第一次渲染之前
    //     console.log(this.props);
        
    // }
    render(){
        // console.log(this.props);
        return <section>
            <h3>系统提示</h3>
            <div></div>
        </section>;
    }
}
ReactDOM.render(<div>
    xiaoke
    <Sum/>
    <Dialog lx={2} con="哈哈哈">
    <span>我是子元素</span>
    </Dialog>
</div>,root);

let obj = {
    type:'div',
    props:{
        children:[
            'xiaoke',
            {
                type:Dialog,
                props:{
                    lx:2,
                    children:null
                }
            }
        ]
    }
}
```