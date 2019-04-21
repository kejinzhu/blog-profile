---
title: react学习笔记(二)
copyright: true
date: 2018-09-05 19:52:08
categories: react
tags:
    - react
    - 脚手架create-react-app
---

## create-react-app的使用

> npm install create-react-app -g

把模块安装在全局环境下(目的：可以使用命令),MAC电脑安装的时候，前面需要加sudo,否则没有权限

> create-react-app [项目名称]

基于脚手架，创建出一个基于react的自动化/工程化项目目录，和npm发包时候的命名规范一样,项目名称中不能出现：大写字母、中文、特殊符号(-或者_是可以的)等

### [脚手架生成目录中的一些内容]

- node_modules ：当前项目中依赖的包都安装在这里
   - .bin：本地项目中可执行命令，在package.json的scripts中配置对应的脚本即可(其中有一个就是:react-scripts命令)
- public：存放的是当前项目的HTML页面(单页面应用放一个index.html即可,多页面根据自己需求放置需要的页面) 

index.html文件解析

```html
<link rel="manifest" href="%PUBLIC_URL%/manifest.json">
<link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">
```

在react框架中，所有的逻辑都是在JS中完成的(包括页面结构的创建),如果想给当前的页面导入一些CSS样式或者image图片等内容,我们有两种方式：

1、在JS中基于ES6 module模块规范,使用import导入，这样webpack在编译合并JS的时候，会把导入的资源文件等插入到页面的解构中(绝对不能在JS管控的解构中通过相对目录./或者../导入资源，因为在webpack编译的时候，地址就不再之前的相对地址了)

2、如果不想在JS中导入(JS中导入的资源最后都会基于webpack编译)，我们也可以把资源手动的在HTML中导入，但是HTML最后也要基于webpack编译，导入的地址也不建议写相对地址。而是使用%PUBLIC_URL%写成绝对地址

- src：项目结构中最主要的目录，因为后期所有的JS、路由、组件等都是放到这里面(包括需要编写的CSS或者图片等)
  - index.js是当前项目的入口文件
- .gitignore：git提交的时候,忽略文件的配置项
- package.json：当前项目的配置清单
  - "dependencies": {
    "react": "^16.4.2",
    "react-dom": "^16.4.2",
    "react-scripts": "1.1.5"
    } 
基于脚手架生成工程目录，自动帮我们安装了三个模块：react/react-dom/react-scripts
react-scripts集成了webpack需要的内容：

- babel一套
- css一套
- eslint一套
- webpack一套
- 其他的

没有less/sass的处理内容(项目中使用less，我们需要自己额外的安装)

```javascript
"scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
}
```

可执行的脚本"npm run start/yarn start"

- start(开发环境 )：开发环境下,基于webpack编译处理,最后可以预览当前开发的项目成果(在webpack中安装了webpack-dev-server插件,基于这个插件会自动创建一个web服务[端口号默认是3000],webpack会帮我们自动打开浏览器,并且展示我们的页面,并且能够监听我们代码的改变，如果代码改变了，webpack会自动重新编译，并且刷新浏览器来完成渲染)

- build(生产环境)：项目需要部署到服务器上，我们先执行build，把项目整体编译打包(完成后会在项目中生成一个build文件夹,这个文件夹中包含了所有编译后的内容，我们把它上传到服务器即可);而且在服务器部署的时候，不需要安装任何模块了(因为webpack已经把需要的内容都打包到一个JS中了)

----------
## React脚手架的深入剖析
`create-react-app`脚手架为了让解构目录清晰，把安装的`webpack`及配置文件都集成在了`react-scripts`模块中,放到了`node_modules`中

但是真实项目中，我们需要在脚手架默认安装的基础上，额外安装一些我们需要的模块，例如：`react-router-dom/axios...`,再比如：`less/less-loader...`

情况1：如果我们安装其他的组件，但是安装成功后，不需要修改webpack配置项，此时，我们直接安装，并且调取使用即可

情况2：我们安装的插件是既有webpack处理的：也就是需要把安装的模块配置到webpack当中(重新修改webpack配置项了)

=>首先需要把隐藏到node_modules中的配置项暴露到项目中

> npm run eject

首先会提示确认是否执行eject操作，这个操作是不可逆的，一旦暴露出来配置项，就无法再隐藏撤回了。

如果当前的项目是基于Git管理，在执行eject的时候，如果没有提交到历史区的内容，需要先提交到历史区，然后再eject才可以，否则报错：This git repository has untracked files or uncommited changes...

=>再去修改对应的配置项即可

一旦暴露后，项目目录当中多了两个文件夹：

- config：存放的是webpack的配置文件

webpack.config.dev.js：开发环境下的配置项(yarn start)
webpack.config.prod.js：生产环境下的配置项(yarn build)

- scripts：存放的是可执行脚本的JS文件

start.js  `yarn start`执行的就是这个JS
build.js  `yarn build`执行的就是这个JS

package.json中的内容也改了

[举个例子：需要配置less]

> cnpm install less less-loader

less是开发和生产环境下都需要配置的

我们预览项目的时候，也是先基于`webpack`编译，把编译后的内容放到浏览器中运行，所以，如果项目中使用了`less`，我们需要修改`webpack`的配置项，在配置项中 加入`less`的编译工作，这样后期预览项目，首先基于`webpack`把`less`编译为`css`,然后呈现在页面中。

> set HTTPS=true && npm run start 开启HTTPS协议模式(设置环境变量HTTPS的值)

原理是：设置环境变量HTTPS的值

> set PORT=63343&& npm run start

修改端口号














