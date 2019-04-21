---
title: react学习笔记(十四)
copyright: true
date: 2018-09-07 11:39:42
categories: react
tags:
    - react学习笔记
    - react-复合组件信息传递(父传子的两种方式)
---

## 复合组件：父组件嵌套子组件

[传递信息的方式]

- A、父组件需要把信息传递给子组件

调取父组件的时候,把信息基于属性的方式传递给子组件(子组件props中存储传递的信息);这种方式只能父组件把信息传递给子组件,子组件无法直接把信息传递给父组件,也就是属性传递信息是反向传递的

1、基于属性传递

```javascript
export default class Vote extends React.Component{
    render(){
        let {title,count}=this.props;
        return <section>
        <VoteHead title={title}/>
        <VoteBody/>
        <VoteFooter/>
        </section>
    }
}
```

2、基于上下文传递

父组件先把需要传给后代元素(包括孙子元素)没用的信息都设置好(设置在上下文中),后代组件需要用到父组件中的信息,主动去父组件中调取使用即可

在父组件中：

- 1、设置子组件的上下文属性类型

static childContextTypes = {};

- 2、获取子组件上下文(设置子组件的上下文属性信息)

getChildContext()

```javascript
static childContextTypes = {
    // 设置上下文中信息的类型
    n:PropTypes.number,
    m:PropTypes.number
};
getChildContext(){
    // return 的是啥,相当于往上下文当中放了啥
    let {count:{n=0,m=0}}=this.props
    return {
        n,
        m
    }
}

<!-- 子组件中设置使用传递进来的上下文类型:设置哪个类型，子组件上下文当中才有哪个属性,不设置的是不允许使用的：this.context.xxx -->

<!-- 指定的上下文属性类型需要和父组件中指定的类型保持一致，否则报错 -->

<!-- 子组件主动获取需要的信息：类型需要和设置的一样,否则报错，并且你需要啥就使用啥即可 -->
static childContextTypes = {
    n:PropTypes.number,
    m:PropTypes.number
}
```

### 属性VS上下文

- 1、属性操作起来简单,子组件是被动接收传递的值(组件内的属性是只读的).只能父传子(子传父不行,父传孙也需要处理：父传子、子再传孙)
- 2、上下文操作起来相对复杂一些,子组件是主动获取使用的(子组件是可以修改获取到的上下文信息的,但是不会影响到父组件中的信息,其他的组件不受影响),一旦父组件设置了上下文信息,它后代组件都可以直接拿来用，不需要一层层传递

## 平行组件：兄弟组件或者毫无关系的组件

### 方案一：让两个平行组件或者毫无关系的两个组件

父：Parent
子：A/B
父组件中有一些信息,父组件把一个方法传递给A,A中把方法执行(方法执行修改父组件信息值),父组件再把最新的信息传递传递给B即可,等价于A操作,影响了B

### 方案二：基于redux来进行状态管理,实现组件之间的信息传输(常用方案)

redux可以应用在任何的项目中(VUE/JQ/React的都可以),React-redux才是专门给react开发的

统一管理状态的容器：每个组件中都可以操作这个容器

当容器中的信息发生改变,可以通知所有用到状态信息的组件重新渲染

直接修改容器中的状态信息是一个不好的操作：A把状态改了(任何组件都可以直接修改),以后获取的状态信息一旦不是我们想要的,都不知道在哪修改的,不方便项目的维护！
更好的方案：带管理员redux的统一状态管理=>redux的核心思想所在