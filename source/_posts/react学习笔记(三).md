---
title: react学习笔记(三)
copyright: true
date: 2018-09-06 20:23:27
categories: react
tags:
    - react学习笔记
    - JSX语法渲染机制
---

## react & react-dom

### [渐进式框架]

一种最流行的框架设计思想，一般框架中都包含很多内容，这样导致框架的体积过于臃肿,不利于页面的优化，拖慢加载的速度。真实项目中，我们使用一个框架,不一定用到所有的功能,此时我们应该把框架的功能进行拆分，用户想用什么，让其自己自由组合即可。

### 全家桶：渐进式框架N多部分组合
Vue全家桶：vue-cli/Vue/vue-router/vuex/axios(fetch)/vue element(vant)

react全家桶：create-react-app/react/react-dom/react-router/redux/react-redux/aioxs/ant/dva/saga/mobx...

### react:react框架的核心部分,提供了Component类可以供我们进行组件开发，提供了钩子函数(生命周期函数：所有的生命周期函数其实都是基于回调函数来完成的)

#### 生命周期：
##### 基于流程

- constructor
- componentWillMount挂载前
- render挂载
- componentDidMount挂载后

##### 状态更新触发

- shouldComponentUpdate
- componentWillUpdate
- componentDidUpdate

##### 属性更新触发

- componentWillReceiveProps在父组件把子组件的属性发生改变后

##### 卸载触发

- componentWillUnmount


### react-dom:把JSX语法(react独有的语法)渲染为真实DOM(能够放到页面中展示的解构都叫做真实DOM)的组件

ReactDOM.render([JSX],[CONTAINER],[CALLBACK]):把JSX元素(虚拟DOM元素)渲染到页面中

#### JSX语法的使用细节

* JSX:REACT虚拟元素
* COMTAINER：容器，我们想把元素放到页面中的哪个容器中
* CALLBACK：当把内容放到页面中呈现触发的回调函数
* JSX：REACT独有的语法，JAVASCRIPT+XML(HTML),和我们自己之前拼接的HTML字符串类似，都是把HTML结构代码和JS代码或者数据混合在一起了,但是它不是字符串
* 1.不建议我们把JSX直接渲染到body当中,而是放在自己创建的容器中,一般我们都放在一个ID为ROOT的DIV中即可
* 2.在JSX中出现的{}是存放JS的,但是要求JS代码执行完成需要有返回结果(JS表达式)
 - =>不能直接放一个对象数据类型的值(对象(除了给style赋值)、数组(数组中如果没有对象都是基本数据类性质或者是JSX元素,这样是可以的)、函数都不可以)
 - =>可以是基本类型的值(布尔类型什么都不显示、null、undefined也是JSX元素，代表的是空)
 - =>循环判断的语句都不支持，但是支持三元运算符
* 3.循环创建JSX元素(一般都基于数组的map方法完成迭代),需要给创建的元素设置唯一的KEY值(当前本次循环内唯一即可)
* 4.只能出现一个根元素
* 5.给元素设置样式类用的是className而不是class
* 6.style中不能直接的写样式字符串,需要基于一个样式对象来遍历赋值
```javascript
let data = 'xiaoke'; 
ReactDOM.render(<div id="box" >helloworld{data}</div>,root,()=>{
    let oBox = document.querySelector("#box");
    console.log(oBox.innerHTML);
});
``` 

不能直接放一个对象数据类型的值(对象、数组(数组中如果没有对象都是基本数据类性质或者是JSX元素,这样是可以的)、函数都不可以)

```javascript
let data = [{
    name:'张三',
    age:22
},
{
    name:'李四',
    age:23
}];
ReactDOM.render(<div id="box">hello world!{data.toString()}</div>,root);
```

循环判断的语句都不支持，但是支持三元运算符

```javascript
let data = [{
    name:'张三',
    age:22
},
{
    name:'李四',
    age:23
}];
ReactDOM.render(<div id="box">hello world!{
    data?"ok":'no'
}</div>,root);
```

```javascript
let data = [{
    name:'张三',
    age:22
},
{
    name:'李四',
    age:23
}];
ReactDOM.render(<div id="box">hello world!{
    data.map((item,index)=>{
        return '@';
    })
}</div>,root);
```

循环创建JSX元素(一般都基于数组的map方法完成迭代),需要给创建的元素设置唯一的KEY值(当前本次循环内唯一即可)

```javascript
let data = [{
    name:'张三',
    age:22
},
{
    name:'李四',
    age:23
}];
ReactDOM.render(<div id="box">hello world!<ul>
    {
    data.map((item,index)=>{
        let {name,age} = item;
        return <li key={index}><span>{name}</span><span>{age}</span></li>;
    })
}
</ul></div>,root);
```

#### JSX语法的渲染过程

把JSX(虚拟DOM)变为真实DOM(面试必问)

##### JSX渲染的机制

- 1、基于babel中的语法解析模块(babel-preset-react)把JSX语法编译为React.createElement(...)结构
- 2、执行React.createElement(type,props,children),创建一个对象(虚拟DOM)
  
  - key:null
  - props:{id:'title',          className:'title',
    children:'kjx',
    style:...}
  - type:'h1' 存的是当前的标签
  - ref:null
  - __proto__:指向Object.prototype

- 3.ReactDOM.render(JSX语法最后生成的对象,容器),基于render方法把生成的对象动态创建为DOM圆度插入到指定的容器中


```javascript
let root = document.querySelector("#root");
let styleObj = {color:'red'};
render(<h1 id="title" className="title" style={styleObj}>kjx</h1>,root);
```

```javascript
React.createElement({
    'h1',
    {id:'title',
    className:'title',
    style:styleObj
    },
    '\u73EO\u57F9\u8BAD'->kjx
})
```

自己写一个JSX

1.创建一个对象(默认有四个属性type/props/ref/key),最后要把这个对象返回
2.根据传递的值修改这个对象

- type=>传递的type
- props=>需要做一些处理:大部分传递props中的属性都赋值给了对象props，有一些比较特殊
 - -> 如果是ref或者key，我们需要把传递的props中的这两个属性值，给创建对象的两个属性，而传递的props中把这两个值删除掉
 - -> 把传递的children作为新创建对象的props中的一个属性

createElement方法

```javascript
function createElement(type,props,children) {
    props=props||{};//处理props
    let obj = {
        type:null,
        props:{
            children:''
        },
        ref:null,
        key:null
    }
    //=>用传递的type和props覆盖原有的默认值
    //obj = {...obj,type,props};//=>{type:type,props:props}
    obj = {...obj,type,props:{...props,children}};
    //=>把ref和key提取出来(并且删除props中的属性)
    'key' in obj.props?(obj.key=obj.props.key,obj.props.key=undefined):null;
    'ref' in obj.props?(obj.ref=obj.props.ref,obj.props.ref=undefined):null;
    return obj;
}
createElement('h1',{id:'title',
className:'title',
style:{color:'red'},
'key':12,
'ref':23
},'\u73EO\u5CF0\u8BAD');
```

render方法：把创建的对象生成对应的DOM元素，最后插入到页面当中

```javascript
function render(obj,container,callback) {
    let {type,props} = obj||{},
        newElement = document.createElement(type);

    for (let attr in props) {
        // 不是私有的，直接结束遍历
        if(!props.hasOwnProperty(attr)){break};
        // 如果当前属性没有值，直接不处理，跳到下一次循环
        if(!props[attr]){continue};
        let val = props[attr];
        //=>className的处理
        if(attr==='className'){
            newElement.setAttribute('class',val);
            continue
        };

        //=>style处理
        if(attr==='style'){
            if(val===''){continue;}
            for (let styleKey in val) {
                if (val.hasOwnProperty(styleKey)) {
                    newElement['style'][styleKey] = val[styleKey];
                }
            }
            continue;
        }

        //=>处理children
        if(attr==='children'){
            if(typeof val === 'string'){
                let text = document.createTextNode(val);
                newElement.appendChild(text);
            }
            continue;
        }

        //=>

        //=>基于setAttribute可以让设置的属性表现在HTML的结构上
        newElement.setAttribute[attr] = val;
    }
    container.appendChild(newElement);
    callback&&callback();
}

render(objJSX,root,()=>{
    console.log('ok~');
    
})
```

