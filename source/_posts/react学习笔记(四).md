---
title: react学习笔记(四)
copyright: true
date: 2018-09-07 11:39:42
categories: react
tags:
    - react学习笔记
    - react函数式组件的基础语法
---

## react组件

不管是Vue还是react框架,设计之初都是期望我们按照"组件/模块管理"的方式来构建程序的，也就是把一个程序划分为一个个的组件来单独处理

### [优势]：

- 1、有助于多人协作开发
- 2、我们开发的组件可以被复用

...

### react中创建组件有两种方式：

- 1、函数声明式组件

component/Dialog.js

每一个组件中都要导入react,因为需要基于它的createElement把JSX进行解析渲染

1.函数执行的返回结果是一个新的JSX(也就是当前组件的JSX结构)
2.props变量存储的值是一个对象,包含了调取组件时候传递的属性值(不传递是一个空对象)

```javascript
import React from "react";

//=>每一个组件中都要导入react,因为需要基于它的createElement把JSX进行解析渲染
/**
 * 函数式声明组件
 * 1.函数执行的返回结果是一个新的JSX(也就是当前组件的JSX结构)
 * 2.props变量存储的值是一个对象,包含了调取组件时候传递的属性值(不传递是一个空对象)
 */

export default function Dialog(props) {
    let { con, lx = 0 ,children} = props,
        title = lx === 0 ? '系统提示' : '系统警告';
    //=>children:可能有可能没有,可能只是一个值,也可能每一项是一个字符串,也可能是一个对象等(都代表双闭合组件中的子元素)    
    return <section>
        <h2>{title}</h2>
        <div>{con}</div>
        {/*把属性中传递的子元素放到组件中的指定位置*/}
        {/* {{children}} */}
        {/*也可以基于react中提供的专门遍历children的方法来完成遍历操作*/}
        {
            React.Children.map(children,item=>item)
        }
    </section>;
};
```

index.js

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import Dialog from "./component/Dialog";
//从react-dom当中导入一个ReactDOM,逗号后面的内容时把ReactDOM这个对象进行解构import {render} from 'react-dom';

// React.createElement(type,props,children)

let root = document.querySelector("#root");
ReactDOM.render(<div>
    {/*加注释：调取组件,只需要把组件当做一个标签调取使用即可
    单闭合和双闭合都可以*/}
    <Dialog con='哈哈哈'/>
    <Dialog con='嘿嘿嘿' lx={2}>{/*属性值不是字符串，我们需要使用大括号包起来*/}
    <span>1</span><span>2</span></Dialog>
</div>,root);
```

- 2、基于继承component类来创建组件

src->component：这个文件夹中存放的就是开发的组件

### 函数式组件的渲染机制

知识点：createElement在处理的时候,遇到一个组件,返回的对象中：type就不再是字符串标签名了,而是一个函数(类),但是属性还是存在props中

```javascript
{
    type:Dialog,
    props:{
        lx:1,
        con:'xxx',
        children:一个值或者一个数组
    }
}
```

> render渲染的时候，我们需要做处理：

首先判断type的类型,如果是字符串,就创建一个元素标签,如果函数或者类,就把函数执行，把props中的每一项(包含children)传递给函数

在执行函数的时候，把函数中return的JSX转换为新的对象(通过createElement),把这个对象返回;紧接着render按照以往的渲染方式,创建DOM元素,插入到指定的容器中即可

