---
title: react学习笔记(十二)
copyright: true
date: 2018-09-07 11:39:42
categories: react
tags:
    - react学习笔记
    - react-复合组建之间的信息传递
---
## 符合组件之间的信息传递
### 父组件把信息传递给子组件

基于属性传递即可(而且传递是单方向的:只能父亲通过属性把信息给儿子,儿子不能直接把信息作为属性传递给父亲)

<!-- more -->
后期子组件如果信息需要修改：可以让父组件传递给子组件的信息发送变化(子组件接收的属性发生变化,组件会重新渲染=>触发componentWillReceiveProps钩子函数)

只要实现点击子组件body中按钮的时候,可以修改父组件panel的状态信息(n).panel的状态改变，panel会重新的执行render渲染,而重新执行render的时候,会把最新的状态值作为属性传递给子组件head,head组件接收的属性值发生改变,head组件也会重新渲染

### 子组件修改父组件中的属性

类似于这种"子改父"的操作,我们需要使用一下技巧完成：

- 1、把父组件中的一个方法,作为属性传递给子组件
- 2、在子组件中,把基于属性传递进来的方法,在合适的时候执行(相当于在执行父组件中的方法:而这个方法中完全可以操作父组件中的信息)

实现过程

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import 'bootstrap/dist/css/bootstrap.css';
import PropTypes from "prop-types";
// head组件
class Head extends React.Component{
    constructor(){
        super();
    }
    render(){
        return <div className="panel-heading">
        {/* 子组件通过属性获取父组件传递的内容 */}
            <h3>点击次数:{this.props.count}</h3>
        </div>
    }
}
// bedy
class Body extends React.Component{
    constructor(){
        super();
    }
    render(){
        return <div className="panel-body">
            <button className="btn btn-success" onClick={this.props.callBack}>点我啊</button>
        </div>
    }
}
class Panel extends React.Component{
    constructor (){
        super();
        this.state={
            n:0
        }
    }
    render(){
        return <section className="panel panel-default" style={{
            width:'50%',
            margin:'0 auto'
        }}>
        {/* 父组件在调取子组件的时候，把信息通过属性传递给子组件 */}
            <Head count={this.state.n}></Head>
            {/* 父组件把自己的一个 方法基于属性传递给子组件，目的是在组件中执行这个方法 */}
            <Body callBack={this.fn}></Body>
        </section>
    }
    fn = ()=>{
        // 修改panel的状态信息
        
        this.setState({
            n:this.state.n+1
        })
    }
}

ReactDOM.render(<Panel/>,root);
```


