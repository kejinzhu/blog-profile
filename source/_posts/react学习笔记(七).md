---
title: react学习笔记(七)
copyright: true
date: 2018-09-07 11:39:42
categories: react
tags:
    - react学习笔记
    - react组件中的属性管理
---
介绍react中的属性管理
<!-- more -->
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import PropTypes from "prop-types";
function Sum() {
    console.log(this);//undefined
    return <div>
        函数声明~
    </div>
}
class Dialog extends React.Component{
    constructor(props){//props,context,updater
        super(props);
    }
    render(){
        // this.props.con = '嘿嘿嘿';会报错,组件中的属性是调取组件的时候(创建类实例的时候)传递给组件的信息,而这部分信息是"只读的"(只能获取不能修改)=>组件的属性是只读的"
        let {lx,con} = this.props;
        return <section>
            <h3>{lx||'系统提示'}</h3>
            <div>{con}</div>
        </section>;
    }
    componentDidMount(){
        //渲染完成
    }
    // this.props这个属性是只读的,我们无法再方法中修改它的值,但是可以给其设置默认值或者设置一些规则(例如：设置是否是必须传递的以及传递值得类型等)
    static defaultProps = {
        lx:'系统提示'
    };
    // prop-types是Facebook公司开发的一个插件，基于这个插件我们可以给组件传递的属性设置规则(设置的规则不会影响页面的渲染，但是会在控制台抛出警告)
    static propTypes = {
        //con:PropTypes.string//=>传递的内容需要是字符串
        con:PropTypes.string.isRequired//=>不仅传递的内容时字符串并且还必须传递
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
