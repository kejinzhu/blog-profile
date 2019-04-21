---
title: react学习笔记(八)
copyright: true
date: 2018-09-07 11:39:42
categories: react
tags:
    - react学习笔记
    - react组件中的状态(数据驱动思想)
---

react中的组件有两个非常重要的概念：

- 1、组件的属性：(只读)调取组件的时候传递进来的属性
- 2、组件的状态：[读写]自己在组件中设定和规划的(只有类声明式组件才有状态的管控，函数式组件声明不存在状态管理)
  - 组件状态类似于Vue中的数据驱动：我们数据绑定的时候是基于状态值绑定，当修改组件状态后，对应的JSX元素也会跟着重新渲染(差异渲染：只把数据改变的部分重新渲染;基于DOM-DIFF算法完成)
  - 当代框架最核心的思想就是：“数据操控视图(视图影响数据)”,让我们告别别JQ手动操作DOM的时代，我们以后只需要改变数据，框架会帮我们重新渲染视图,从而减少直接操作DOM(提高性能，也有助于开发效率)

<!-- more -->

## 函数式声明组件

所谓函数式组件是静态的：和执行普通函数是一样的，调取一次组件，就把组件中的内容获取到，插入到页面中，如果不重新调取组件，显示的内容时不会发生任何改变

真实项目之一调取组件，组件中的内容不会再改变的情况下，我们才能使用函数式组件

```javascript
function Clock() {
    return <div>
        <h3>当前北京时间是：</h3>
        <div style={{color:'red',fontSize:'bold'}}>{new Date().toLocaleString()}</div>
    </div>;
}

setInterval(()=>{
    // 每个1000ms重新调取组件，然后渲染到页面中
    ReactDOM.render(<Clock/>,root);
},1000)
```

## 类声明组件

修改组件的状态：

* 1、修改部分状态：会用我们传递的对象和初始化的state进行匹配，只把我们传递的属性进行修改，没有传递的依然保留原始的状态信息(部分状态修改)
* 2、当状态修改完成，会通知react把组件JSX中的元素重新进行渲染

```javascript
class Clock extends React.Component {
    constructor() {
        super();
        // 初始化组件的状态:要求我们在constructor中把后面使用的状态信息全部初始化一下(这是约定俗称的语法规范)
        this.state= {
            time:new Date().toLocaleString()
        }
    }
    render() {
        return <div>
            <h3>当前北京时间是：</h3>
            <div style={{ color: 'red', fontSize: 'bold' }}>
            {/* 获取组件的状态信息 */}
                {this.state.time}
            </div>
        </div>;
    }
    componentDidMount(){
        //生命周期函数之一：第一次组件渲染完成后出发(我们只需要间隔1s把state状态中的time数据改变，这样react会自动帮我们把组件中的部分内容进行重新渲染)
        setInterval(()=>{
            // react中虽然下面方式可以修改状态，但是并不会通知react重新渲染页面，所以不要这样操作和修改状态
            // this.state.time = new Date().toLocaleString();

            /**
             * 修改组件的状态：
             * 1、修改部分状态：会用我们传递的对象和初始化的state进行匹配，只把我们传递的属性进行修改，没有传递的依然保留原始的状态信息(部分状态修改)
             * 2、当状态修改完成，会通知react把组件JSX中的元素重新进行渲染
             */
            this.setState({
                time:new Date().toLocaleString()
            },()=>{
                //当通知react把需要重新渲染的JSX元素渲染完成后，执行的回调操作(类似于生命周期函数中的componentDidUpdate,项目中一般使用钩子函数而不是这个回调)
                // 设置的回调函数的原因：setState是异步的操作
            })
        },1000)
    }
}

```