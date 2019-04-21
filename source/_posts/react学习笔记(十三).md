---
title: react学习笔记(十三)
copyright: true
date: 2018-09-07 11:39:42
categories: react
tags:
    - react学习笔记
    - react-基础知识复习
---
## react基础知识复习：(一)

- 1、react是Facebook公司开发的一款MVC版的JS框架
 - MVC：model(数据层) view(视图层) controller(控制层)
 - 核心思想：通过数据的改变来影响视图的渲染(数据驱动的思想)
- 2、基于脚手架create-react-app,快速构建一个react工程项目结构
 - 自动安装react的核心组件：react/react-dom
 - 自动安装webpack,并且完成相关的配置：

 <!-- more -->

 =>区分了开发环境和生产环境
 =>安装babel以及对应的语言解析包,可以把react和ES6进行编译处理
 =>安装css/style/file等加载器,处理css等合并压缩的问题
 =>安装了ES-Lint,可以进行代码检查
 =>安装了很多的插件，可以实现了JS和CSS以及HTML的分离,打包、压缩等
 =>安装了webpack-dev-server,可以在开发环境下,编译后自动创建服务,打开浏览器,当代码修改后，自动保存编译,页面自动刷新渲染等

[使用脚手架]
```javascript
<!-- 把脚手架安装到全局环境下，以后应用命令操作，完成项目结构的搭建 -->
npm install create-react-app
<!-- 创建项目结构目录 -->
create-react-app +项目名(遵循npm发包规范：名字只能是/^[a-z0-9_-]$/)

特点是：如果当前电脑安装了yarn ,创建工程目录的时候，走的是yarn 安装,yarn和npm主体相同,但是处理起来还有一定的区别,所以我们以后继续向工程中安装模块以及执行配置脚手架打包编译的时候，尽可能使用yarn ,不建议和npm混用
```

[工程化目录]

|-node_modules
  |-.bin所有在本地可执行的命令脚本(react-script.cmd)
  |...
|-package.json当前项目的配置清单
|-public 存放的是当前项目的HTML页面(有可能放一部分静态资源)
  |-index.html
  |...
  |-src存放的项目需要的所有的js或者静态资源(包括组件、store、路由、数据模型、ajax请求等等内容,我们开发的内容基本上所有的东西都在src中写)
  |-index.js当前项目的入口文件
  |...

[暴露webpack配置项]

脚手架构建项目的时候，为了解构的美化,把所有的webpack配置等都隐藏到了node_modules中(react-scripts中),真实项目中,我们经常基于脚手架构建的解构自己在安装配置一些信息(例如：less处理的配置),此时我们需要把配置项暴露出来，

```javascript
yarn eject //此项是不可以逆转的(而且操作之前需要把所有修改的文件提交大Git仓库中)

//目录中多了
//|-config
  //|-webpack.config.dev.js:开发环境下的wp配置
  //|-webpack.config.prod.js:生产环境下的wp配置
  //|-path.js基本配置项(包含项目的入口信息)
//|-scripts
  //|-start.js/build.js.test.js当我们执行yarn start/build/test的时候，执行的就是这三个JS中的一个
```  

[可执行的本地脚本命令]

```javascript
yarn start //开发依赖
```

=>创建一个端口号为3000,协议为http的web服务
=>打开浏览器,预览我们正在开发的项目
=>当项目文件修改的时候，自动重新编译,浏览页面自动刷新,展示最新的效果

[mac/linux]

```javascript
HTTPS=true yarn start
PORT=1234 yarn start
```

```javascript
yarn build
```

=>生成一个build文件夹,存放最后打包的文件
=>基于webpack.config.prod.js,把项目进行编译打包
=>部署上线的时候,只需要把build中的内容发布即可

[基于脚手架配置less]

安装less和对应的加载器

```javascript
yarn add less less-loader
```

修改开发和生产环境下的webpack配置

---------------
## react基础知识复习(二)：
- 1、react是基于独有的JSX语法实现视图(数据和HTML)渲染的
- 2、JSX语法
 - A、JSX语法的渲染使用的是ReactDOM.render

 ```javascript
 ReactDOM.render([JSX元素],[指定的容器],[回调函数：当我们把JSX放到指定容器内,触发执行的函数])
 ```

 - B、JSX=JAVASCRIPT+XML(HTML)

 ```javascript
 1、不推荐存放的容器是body(写body会报错),一般都是使用我们自己创建的一个根元素(例如：创建#root的div容器)

 
 ReactDOM.render(<h3>小柯</h3>,root)

 2、不允许出现两个 `根`元素,如果需要绑定复杂的解构,最外层嵌套一个容器作为根元素即可
  ReactDOM.render(<div><h3>小柯</h3><span></span></div>,root)

 3、把数据渲染到JSX中 (不是嵌入到元素的属性中,而是正常的内容中) 
 =>可以嵌入变量或者直接的数据值

 let name="xxx";
 ReactDOM.render(<div><h3>{name}</h3><span>{'hahah'}</span></div>,root)
 =>不能嵌入对象(代指：{}、/^$/、日期对象、函数、或者数组中的某一项是前面的也不行[一维简单的数据是可以的])

 =>可以嵌入基本类性质(null、undefined、布尔值都是空元素,也就是不显示任何内容)

 =>大括号中可以嵌入JSX表达式(执行JS代码需要有返回结果的)

 循环创建的JSX元素需要设置标识key,并且在当前循环的时候,这个key需要唯一,使用map是因为他有返回值,返回的是替换后的数组

 ReactDOM.render(<ul>
 {
     data.map((item,index)=>{
         return <li key={index}>{item.id}</li>
     })
 }
 </ul>,root);

使用三元运算符解决判断操作(if和switch都不可以)

4、可以给JSX元素设置属性
=>属性值对应大括号中、对象、函数都可以放(也可以放JS表达式)
=>style属性值必须是对象(不能是字符串)
=>class用className来代替

5、JSX语法：JSX元素/react元素/虚拟DOM
react是如何把JSX元素转换为真实的DOM元素并添加到页面中？

=>基于babel/babel-loader/babel-preset-react-app把JSX语法编译为react.createElemnt这种模式
=>1、create-element中至少两个参数,没有上限
 =>第一个：目前是当前元素标签的标签名(字符串)
 =>第二个：属性(没有给元素设置属性则为null)
 =>其他的：当前元素的所有子元素内容(只要子元素是HTML，就会变为新的create-element)

=>2、执行create-element,把传递进来的参数最后处理成为一个对象
{
    type:'标签名',
    props:{
        自己设置的哪些属性对象(但是对于key和ref来说是要提取出来的)
        children:存放自己子元素的(没有资源数就没有这个属性),如果有多个子元素,就可以以数组的形式存储信息
    },
    ref:null,
    key:null
} 
=>3、把生成的对象交给render进行处理：把对象编程DOM元素,插入到指定的容器中
```
--------------
## react基础知识复习(三)
真实项目中我们使用react都是基于组件或者模块开发的

调取组件的时候：babel解析,传递给create-element的第一个参数type不再是字符串标签名,而是一个函数(类),生成的对象中,type也是一个函数,当render渲染的时候会根据type类型做不同的事情(如果是字符串就是创建新元素,如果是函数,就会把函数执行,把返回的JSX对象创建成为新的元素进行渲染),函数执行的时候会把解析处理的对象中的props作为参数传递给组件(函数)

- 1、函数式创建组件

函数式组件声明：创建一个函数、里面返回一个JSX

函数式声明的特点：

1、会把基于create-elment解析出来的对象中的props作为参数传递给组件(可以完成多次调取组件传递不同的信息)
2、一旦组件调取成功,返回的JSX就会渲染到页面上,但是后期不通过重新调取组件或者获取DOM元素操作的方式,很难再把渲染好的组件内容再更改=>函数式组件声明时`静态组件`

```javascript
function Vote(){
    return <section></section>...
}
```

- 2、基于类创建组件(基于继承component类实现的)

基于类创建组件的特点：

1、调取组件相当于创建类的实例(this),把一些私有的属性挂载到实例上了,这样组件内容所有方法中都可以基于实例获取这些值(包括：传递的属性和自己管理的状态)
2、有自己的状态管理,当状态改变的时候,react会重新渲染视图(差异更新：基于DOM-DIFF只把需要重新渲染的部分渲染即可)

=> ref：是react中提供操作DOM的方案

1、给需要操作的元素设置ref(保持一致性,否则会冲突覆盖)
2、在实例上挂载了refs属性

```javascript
class A extends React.Component{
    constructor(){
        super();//=>React.Component.call(this)可以把component中的私有属性继承过来this.props/this.state(this.setState)/this.context/this.ref/this.updater
    }
}
```

-------------

## react基础知识复习(四)

```javascript
let n = 0;
this.setState({
    m:n++
});
```

- 1、生命周期函数

[调取组件]

 - constructor
 - componentWillMount第一次渲染之前
 - render渲染
 - componentDidMount第一次渲染之后

[组件重新渲染：内部状态改变、传递给组件的属性改变]

- 状态改变：
 - shouldComponentUpdate

  =>是否允许组件更新:返回true是允许,返回false则不再继续向下走

 - componentWillUpdate

  =>更新之前:和should一样,方法中通过this.state.xxx获取的还是更新前的状态信息,方法中有两个参数：nextProps/nextState存储的是最新的属性和状态信息

 - render

  =>更新 

 - componentDidUpdate
  
  =>更新之后  

- 属性改变
 
 - componentWillReceiveProps(nextProps,nextState)
  
  =>接收最新属性之前,基于this.props.xxx获取的还是原有的状态信息 
 
 - shouldComponentUpdate
  
  =>是否允许组件更新:返回true是允许,返回false则不再继续向下走
 
 - componentWillUpdate
 
  =>更新之前:和should一样,方法中通过this.state.xxx获取的还是更新前的状态信息,方法中
  有两个参数：nextProps/nextState存储的是最新的属性和状态信息
 
 - render
 
  =>更新 
 
 - componentDidUpdate
 
  =>更新之后   

[组件销毁]

- componentWillUnmount
 
 =>组件销毁之前
 

> 注意：组件的属性是只读的: 只能调取组件的时候传递进来，不能自己在组件内部修改(但是可以设置默认值和规则)

> 组件的状态是可读写的：状态改变会引发组件的重新更新(状态是基于setState改变)

> 组件实例上可以放一些信息：这些信息只是为了方便在组件内任意方法中获取和使用的

> 实例上挂在的refs：就是用来操作DOM的

> 实例上挂在的context：是用来实现组件之间信息传递的

--------------









