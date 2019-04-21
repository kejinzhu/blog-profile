---
title: '[React]-NAVlink和Link组件'
copyright: true
date: 2018-10-04 15:01:48
categories: react
tags:
    - react
    - NavLink和Link组件
---
## link：
是react-router中提供的路由切换组件,基于它可以实现点击的时候路由切换
TO[string]:跳转到指定的路由地址
TO[object]:可以提供一些参数配置项(和redirect类似)
{
    pathname:跳转地址
    search:问号传参
    state:基于这种方式传递信息
}
replace:false 是替换history stack中当前的地址(true),还是追加一个新的地址(false)

原理：基于link组件渲染,渲染后的结果就是一个a标签,to对应的信息最后变为href中的内容

---------------
react router中提供的组件都要在任何一个router(hash-router)包裹的范围内使用

## NavLink:和link类似,都是为了实现路由切换跳转,不同在于,NavLink组件在当前页面hash和组件对应地址相吻合的时候,会默认给组件加一个active样式，让其有选中态

- activeClassName:把默认加的active样式类改为自己设定的饿
- activeStyle:给匹配的这个navlink设置行内样式

```javascript
exact&&strict控制匹配的时候是否是严格匹配
isActive:匹配后执行对应的函数
```

```javascript
<NavLink to="/custom" activeClassName=""/>//最后也会转换为A标签,如果当前页面的hash地址和此组件中的to地址匹配了,则会给渲染后的A标签设置默认的样式类:active
```
