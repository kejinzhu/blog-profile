---
title: '[React]-redux的中间件'
copyright: true
date: 2018-10-04 16:08:29
categories: react
tags:
    - react
    - redux中的中间件
---
# redux中间件
- redux-logger:能够在控制台清晰的展示出当前redux操作的流程和信息(原有状态、派发信息、修改后的状态信息)
- redux-promise：在dispatch派发的时候支持promise操作
- redux-chunk:处理异步dispatch派发
<!-- more -->

## chunk中间件的使用语法：
在指定执行派发任务的时候,等待3000ms后再派发
dispatch都传递给我们了,我们想什么时候派发，自己搞定即可
## promise中间件的语法
传递给reducer的payload需要等待promise成功,把成功的结果传递过去



