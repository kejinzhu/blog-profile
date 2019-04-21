---
title: react学习笔记(九)
copyright: true
date: 2018-09-07 11:39:42
categories: react
tags:
    - react学习笔记
    - react-投票案例(运用数据驱动的思想)
---

## JSX中的事件绑定

```javascript
render(){
    return <button className="btn btn-success" onClick={this.support}>支持
    </button>
}
support(ev){
    //=>this:undefined(不是我们理解的操作的元素)
    //=>ev.target:通过事件源可以获取当前操作的元素(一般很少操作,因为框架一般都是数据驱动所有DOM的改变)
}
```

如果能让方法中的this变成当前类的实例就好了，这样就可以操作属性和状态等信息

```javascript
render(){
    //=>this指向实例,bind改变一下this指向
    return <button className="btn btn-success" onClick={this.support.bind(this)}>支持
    </button>
}
support(ev){
    //=>this:undefined(不是我们理解的操作的元素)
    //=>ev.target:通过事件源可以获取当前操作的元素(一般很少操作,因为框架一般都是数据驱动所有DOM的改变)
}
```

每次用bind来写很麻烦，我们来看另外一种写法

render(){
    //=>this指向实例,bind改变一下this指向
    return <button className="btn btn-success" onClick={this.support}>支持
    </button>
}
support=ev=>{
    //=>this:undefined(不是我们理解的操作的元素)
    //=>ev.target:通过事件源可以获取当前操作的元素(一般很少操作,因为框架一般都是数据驱动所有DOM的改变)

    箭头函数：没有this，继承上下文中的this(实例),真实项目中,我们给JSX元素绑定的事件方法一般都是箭头函数,目的是为了保证函数中的this还是实例
}

数据驱动版完整代码：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import 'bootstrap/dist/css/bootstrap.css';
import PropTypes from "prop-types";
class Vote extends React.Component {
    //组件传递的属性是只读的,我们为其设置默认值和相关规则
    static defaultProps = {

    };
    static propTypes = {
        title: PropTypes.string.isRequired
    }
    constructor(props) {
        super(props);
        // init state: 初始化状态
        this.state = {
            n: 0,//=>支持人数
            m: 0//反对人数
        };
    }
    render() {
        let { n, m } = this.state,
            rate = (n + m) === 0 ? '0%' : ((n / (n + m) * 100).toFixed(2) + '%');
        return <section className="panel panel-default" style={{ width: '60%', margin: '20px auto' }}>
            <div className="panel-heading">
                <h3 className="panel-title" style={{ textAlign: 'center' }}>{this.props.title}</h3>
            </div>
            <div className="panel-body">
                支持人数：{n}
                <br />
                <br />
                反对人数：{m}
                <br />
                <br />
                支持率：{rate}
            </div>
            <div className="panel-footer">
                <button className="btn btn-success" onClick={this.support}>支持
                {/* 这么写this变成undefined */}
                </button>
                &nbsp;&nbsp;&nbsp;&nbsp;
                <button className="btn btn-danger" onClick={this.againist}>反对</button>
            </div>
        </section>;
    }
    // 投票：支持,用箭头函数，是this指向实例
    support=ev=>this.setState({n:this.state.n+1});
    // 投票：反对
    againist=ev=>this.setState({m:this.state.m+1})
}
ReactDOM.render(<main>
    <Vote title='世界杯小组赛法国VS秘鲁,法国队必胜' />
    <Vote title="世界杯小组赛阿根廷VS克罗地亚,壮哉我大梅西" />
</main>, root);

```

<!-- DOM操作示例 -->

refs:是react中专门提供通过操作DOM来实现需求的方式，它是一个对象，存储了当前组件中所有设置了ref属性的元素(元素ref属性值是啥,refs中存储的元素的属性名就是啥)

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import 'bootstrap/dist/css/bootstrap.css';
import PropTypes from "prop-types";
class Vote extends React.Component {
    //组件传递的属性是只读的,我们为其设置默认值和相关规则
    static defaultProps = {

    };
    static propTypes = {
        title: PropTypes.string.isRequired
    }
    constructor(props) {
        super(props);
    }
    render() {
        return <section className="panel panel-default" style={{ width: '60%', margin: '20px auto' }}>
            <div className="panel-heading">
                <h3 className="panel-title" style={{ textAlign: 'center' }}>{this.props.title}</h3>
            </div>
            <div className="panel-body">
                支持人数：<span ref='spanLeft'></span>
                <br />
                <br />
                反对人数：<span ref='spanRight'></span>
                <br />
                <br />
                支持率：<span ref='spanRate'></span>
            </div>
            <div className="panel-footer">
                <button className="btn btn-success" onClick={this.support}>支持
                {/* 这么写this变成undefined */}
                </button>
                &nbsp;&nbsp;&nbsp;&nbsp;
                <button className="btn btn-danger" onClick={this.againist}>反对</button>
            </div>
        </section>;
    }
    // 投票：支持,用箭头函数，是this指向实例
    support=ev=>{
        let {spanLeft} = this.refs;
        spanLeft.innerHTML++;
        this.computed();
    }
    // 投票：反对
    againist=ev=>{
        let {spanRight} = this.refs;
        spanRight.innerHTML++;
        this.computed();
    };
    computed = ()=>{
        let {spanLeft,spanRight,spanRate} = this.refs,
        n = parseFloat(spanLeft.innerHTML),
        m = parseFloat(spanRight.innerHTML),
        rate = (n+m)===0?'0%':((n/(n+m)*100).toFixed(2)+'%');
        spanRate.innerHTML = rate;
    }
}
ReactDOM.render(<main>
    <Vote title='世界杯小组赛法国VS秘鲁,法国队必胜' />
    <Vote title="世界杯小组赛阿根廷VS克罗地亚,壮哉我大梅西" />
</main>, root);

```

在react组件中：

- 1、基于数据驱动(修改状态数据,react帮助我们重新渲染视图)完成的组件叫做"受控组件(受数据控制的组件)"
- 2、基于REF操作DOM实现视图更新的，叫做"非受控组件"

=> 真实项目中，建议多使用"受控组件"
