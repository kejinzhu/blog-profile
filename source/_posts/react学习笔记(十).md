---
title: react学习笔记(十)
copyright: true
date: 2018-09-07 11:39:42
categories: react
tags:
    - react学习笔记
    - react-基于表单元素的onchange实现MVVM双向绑定
---
Vue：[MVVM]数据更改视图跟着更改，视图更改数据也跟着更改(双向数据绑定)
react：[MVC]数据更改视图跟着更改(原本是单向的数据绑定,但是我们可以构建出双向的效果)
<!-- more -->

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import 'bootstrap/dist/css/bootstrap.css';
import PropTypes from "prop-types";

class Temp extends React.Component{
    static propTypes = {
        
    }
    constructor(){
        super();
        this.state = {
            text:'柯金珠'
        }
    }
    componentDidMount(){
        setTimeout(() => {
            this.setState({
                text:'kjz'
            })
        }, 1000);
    }
    render(){
        return <section className="panel panel-default">
        <div className="panel-heading">
            <input type="text" name="" id="" className="form-control" value="text" onChange={ev=>{
                //=>在onChange中实现修改状态
                this.setState({
                    text:ev.target.value
                })
            }}/>
        </div>
        <div className="panel-body">
            {text}
        </div>
        </section>
    }
}
ReactDOM.render(<Temp/>,root);

```


