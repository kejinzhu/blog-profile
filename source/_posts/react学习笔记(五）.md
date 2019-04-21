---
title: react学习笔记(五)
copyright: true
date: 2018-09-07 11:39:42
categories: react
tags:
    - react学习笔记
    - 封装Dialog组件
---

自己手动封装的Dialog组件

<!-- more -->

index.js

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

import 'bootstrap/dist/css/bootstrap.css';//我们一般都把程序中的公用样式放到index中导入,这样在其他组件中也可以使用了(webpack会把所有的组件最后都编译到一起,index是主入口)
import Dialog from './component/Dialog';

//2.导入bootstrap,需要导入的是不经过压缩处理的文件，否则无法编译(真实项目中bootstrap已经是过去式,后期项目中使用的都是ant来做)


ReactDOM.render(<main><Dialog content='你很帅哦~'/><Dialog type={2} content="哎呀，有点丑"/>
<Dialog type='登录先' content={<div>
    <input type="text" className="form-control" placeholder="请输入用户名"/>
    <input type="text" className="form-control" placeholder="请输入密码"/>
</div>}/><button className="btn btn-success">登录</button><button className="btn btn-danger">取消</button></main>,root);
```

Dialog.js

```javascript
import React from "react";
export default function Dialog(props) {
    // 自己处理的样式
    let objStyle = {
        width:'50%',
        margin:'0 auto'
    };
    let {type,content,children} = props;
    // 类型处理
    let typeVal = type||'系统提示';
    if(typeof type ==="number"){
        switch (type) {
            case 0:
                typeVal = '系统提示';
                break;
            case 1:
                typeVal = '系统警告';
                break;
            case 2:
                typeVal = '系统错误';
                break;      
        }
    }
    return <section className="panel panel-default " style={objStyle}>
        <div className="panel-heading">
            <h3 className="panel-title">
                系统提示：
        </h3>
        </div>
        <div className="panel-body">
            {content}
        </div>
        {/*如果传入了children，我们把内容放到尾部，不传什么都不显示*/}
        {
            children?<div className="panel-footer">{React.Children.map(children,item=>item)}
        </div>:null}
    </section>
}

```