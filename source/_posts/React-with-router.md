---
title: '[React]-with-router'
copyright: true
date: 2018-10-04 15:28:58
categories: react
tags:
    - react
    - with-router
---
## with-router:
这个方法的意思是把一个非路由管控的组件,模拟成为路由管控的组件

```javascript
<Route path="" component={Nav}/>//受路由管控的组件
withRouter(connect()(Nav));//先把nav基于connect高阶一下,返回的是一个代理组件proxy,把返回的代理组件受路由管控

```
<!-- more -->
### 受路由管控组件的一些特点：

- 1、只有当前页面的哈希地址(/#/...)和路由指定的地址(path)匹配,才会把对应的组件渲染(withRouter是没有地址匹配,都被模拟成为受路由管控的)
- 2、路由切换的原理：凡是匹配的路由,都会把对应的组件内容重新添加到页面中,相反,不匹配的都会在页面中移除掉,下一次重新匹配上,组件需要重新渲染到页面中;每一次路由切换的时候(页面的哈希路由地址改变),都会从一级路由开始重新校验一遍;
- 3、所有受路由管控的组件,在组件的属性props上都默认添加了三个属性;

history：

- 1、push：向池子中追加一条新的信息,达到切换到指定路由地址的目的this.props.history.push("/plan")JS中实现路由切换
- 2、go：跳转到指定的地址(传的是数字 0 当前 -1上一个 -2上两个)
- 3、goback：<=>go(-1)回退到上一个地址
- 4、goforward：<=>go(1)向前走一步
...

match：获取的是当前路由；匹配的一些结果

- 1、params:如果当前路由匹配的是地址路径参数,则这里可以获取传递参数的值
- 2、

location：获取当前哈希路由渲染组件的一些信息

- 1、pathname:当前哈希路由地址/custom/list
- 2、search：当前页面问号传参值:?lx=unsafe
- 3、state：基于redirect/link/navlink中的to,传递的是一个对象,对象中编写的state,就可以在location.state中获取到

#### history stack:历史信息栈(池子)

```javascript
<Redirect to=""/>或者<Link to=""/>或者<NavLink to="">
//可以实现路由切换
```

- 0："/"
- 1: "/custom"

基于<Redirect from="/custom" to="/custom/list" />

如果不加push,是把当前信息替换,"/custom/list"
如果加了push,是产生一条新的记录

- 2："/custom/list"

每一次路由的切换,要不然是替换现有的地址,要不然就是新增一条地址信息,不管怎么样,都会立即切换到新的地址