---
title: node-CommonJS规范
copyright: true
date: 2018-09-04 19:05:51
categories: node
tags:
    - node
    - commonJS规范
---

## node-初级玩家

node本身是基于commonJS模块规范设计的，所以模块是node的组成

- 内置模块：node天生提供给JS调取使用的
- 第三方模块：别人写好的，我们可以基于npm安装使用
- 自定义模块：自己创建的一些模块
<!-- more -->
commonJS模块设计的思想(AMD/CMD/ES6 module都是模块化设计思想)

```
1.commonJS规定：每一个JS都是一个单独的模块(模块是私有的，里面涉及的值和变量以及函数等都是私有的，和其他的JS文件中的内容是不冲突的)
2.commonJS中可以允许模块中的方法相互的调用
B模块中想要调取A模块中的方法
=> A导出
=> B导入

[导出]

commonJS给每一个模块(每个JS)中都设置了内置的变量/属性/方法
module:代表当前这个模块对象[object]
module.exports:模块的这个属性是用来导出当前属性和方法的[object]
exports:是内置的一个`变量`,也是用来导出当前模块属性方法的,虽然和module.exports不是一个东西，但是对应的值是同一个(module.exports=exports值都是对象)

[导入]

require:commonJS提供的内置变量,用来导入模块的(其实导入的就是module.exports暴露出来的东西;导入的值也是[object]类型的;)
```

temp1:

```javascript
let a = 12,
    fn = b=>{
        return a*b;
    };
exports.fn = fn;//=>把当前模块私有的函数
```

temp2:

```javascript
let a = 13,
    fn = b=>{
        return a/b;
    };
let temp1 = require('./temp1');//=>特意指定是当前目录中的某个模块(.js可以省略)require导入的时候，首先把temp1模块中的JS自上而下执行，把exports对应的堆内存导入进来，所以接收到的结果是一个对象(require是一个同步操作：只有把导入的模块代码执行完成，才可以获取值，然后继续执行本模块下面的代码) 
```



