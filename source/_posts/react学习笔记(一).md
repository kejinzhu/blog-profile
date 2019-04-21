---
title: react学习笔记(一)
copyright: true
date: 2018-09-04 20:51:29
categories: react
tags:
    - react
    - 初步
---

React 是Facebook公司研发的一款JS框架(MVC)
<!-- more -->

## React的脚手架
React是一款框架：具备自己开发的独立思想(MVC：model view controller)

- 划分组件开发
- 基于路由的SPA单页面开发
- 基于ES6来编写代码(最后部署上线的时候,我们需要把ES6编译成ES5=>基于babel来完成编译)
- 可能用到less或者sass等，我们也需要使用对应的插件，把他们进行预编译
- 最后为了优化性能(减少http的请求次数)，我们需要把JS或者CSS进行合并压缩
- webpack来完成以上页面组件合并、JS/CSS编译加合并等工作

## 前端工程化开发：

- 基于框架的组件化开发 
- 基于webpack的自动部署

但是配置webpack是一个相对复杂的工作，我们需要自己安装很多的包，还需要自己写相对复杂的 配置文件。。。，如果我们有一个插件， 基于它可以快速构建一套完整的自动化工程项目结构，那么有助于提高开发效率=>"脚手架"

Vue中的脚手架：vue-cli
React的脚手架：create-react-APP



