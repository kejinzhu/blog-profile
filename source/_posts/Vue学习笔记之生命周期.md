---
title: Vue学习笔记之生命周期
copyright: true
date: 2018-09-05 10:36:04
categories: Vue
tags:
    - Vue学习笔记
    - 生命周期
---

## 生命周期
概念：

生命周期：Vue的实例具有生命周期;Vue实例在生成时,会经历一系列的初始化过程;数据的监听,编译模板,实例挂载DOM元素,或者数据更新DOM更新;在执行的过程中,会运行一些叫做 `生命周期` 的钩子函数;在Vue实例生命周期中的特定的时间点内,执行的函数

<!-- more -->

![Vue生命周期图](/images/lifecycle.png)

生命周期钩子函数，不能控制其执行的顺序
beforeCreate  created:创建
beforeMount  mounted:挂载

Vue实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模板、挂载Dom、渲染→更新→渲染、销毁等一系列过程，我们称这是Vue的生命周期。通俗说就是Vue实例从创建到销毁的过程，就是生命周期。

每一个组件或者实例都会经历一个完整的生命周期，总共分为三个阶段：初始化、运行中、销毁。

- 1、实例、组件通过new Vue() 创建出来之后会初始化事件和生命周期，然后就会执行beforeCreate钩子函数，这个时候，数据还没有挂载呢，只是一个空壳，无法访问到数据和真实的dom，一般不做操作

- 2、挂载数据，绑定事件等等，然后执行created函数，这个时候已经可以使用到数据，也可以更改数据,在这里更改数据不会触发updated函数，在这里可以在渲染前倒数第二次更改数据的机会，不会触发其他的钩子函数，一般可以在这里做初始数据的获取

- 3、接下来开始找实例或者组件对应的模板，编译模板为虚拟dom放入到render函数中准备渲染，然后执行beforeMount钩子函数，在这个函数中虚拟dom已经创建完成，马上就要渲染,在这里也可以更改数据，不会触发updated，在这里可以在渲染前最后一次更改数据的机会，不会触发其他的钩子函数，一般可以在这里做初始数据的获取

- 4、接下来开始render，渲染出真实dom，然后执行mounted钩子函数，此时，组件已经出现在页面中，数据、真实dom都已经处理好了,事件都已经挂载好了，可以在这里操作真实dom等事情...

- 5、当组件或实例的数据更改之后，会立即执行beforeUpdate，然后vue的虚拟dom机制会重新构建虚拟dom与上一次的虚拟dom树利用diff算法进行对比之后重新渲染，一般不做什么事儿

- 6、当更新完成后，执行updated，数据已经更改完成，dom也重新render完成，可以操作更新后的虚拟dom

- 7、当经过某种途径调用$destroy方法后，立即执行beforeDestroy，一般在这里做一些善后工作，例如清除计时器、清除非指令绑定的事件等等

- 8、组件的数据绑定、监听...去掉后只剩下dom空壳，这个时候，执行destroyed，在这里做善后工作也可以

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <aaa></aaa>
    </div>

    
    <template id="aaa">
        <div>
            <p class="myp">A组件</p>
            <button @click="destroy">destroy</button>
            <input type="text" v-model="msg">
            <p>msg:{{msg}}</p>
        </div>
    </template>



</body>
<script src="./vue.js"></script>

<script>
    //生命周期：初始化阶段 运行中阶段 销毁阶段
    Vue.component("aaa",{
        template:"#aaa",
        data:function(){
            return {msg:'hello'}
        },
        timer:null,
        methods:{
            destroy:function(){
                this.$destroy()//
            }
        },
        beforeCreate:function(){
            console.log('beforeCreate:刚刚new Vue()之后，这个时候，数据还没有挂载呢，只是一个空壳')           
            console.log(this.msg)//undefined
            console.log(document.getElementsByClassName("myp")[0])//undefined
        },
        created:function(){
            console.log('created:这个时候已经可以使用到数据，也可以更改数据,在这里更改数据不会触发updated函数')
            this.msg+='!!!'
            console.log('在这里可以在渲染前倒数第二次更改数据的机会，不会触发其他的钩子函数，一般可以在这里做初始数据的获取')
            console.log('接下来开始找实例或者组件对应的模板，编译模板为虚拟dom放入到render函数中准备渲染')
        },
        beforeMount:function(){
            console.log('beforeMount：虚拟dom已经创建完成，马上就要渲染,在这里也可以更改数据，不会触发updated')
            this.msg+='@@@@'
            console.log('在这里可以在渲染前最后一次更改数据的机会，不会触发其他的钩子函数，一般可以在这里做初始数据的获取')
            console.log(document.getElementsByClassName("myp")[0])//undefined
            console.log('接下来开始render，渲染出真实dom')
        },
        // render:function(createElement){
        //     console.log('render')
        //     return createElement('div','hahaha')
        // },
        mounted:function(){ 
            console.log('mounted：此时，组件已经出现在页面中，数据、真实dom都已经处理好了,事件都已经挂载好了')
            console.log(document.getElementsByClassName("myp")[0])
            console.log('可以在这里操作真实dom等事情...')

        //    this.$options.timer = setInterval(function () {
        //        console.log('setInterval')
        //         this.msg+='!'  
        //    }.bind(this),500)
        },
        beforeUpdate:function(){
            //这里不能更改数据，否则会陷入死循环
            console.log('beforeUpdate:重新渲染之前触发')
            console.log('然后vue的虚拟dom机制会重新构建虚拟dom与上一次的虚拟dom树利用diff算法进行对比之后重新渲染')         
        },
        updated:function(){
            //这里不能更改数据，否则会陷入死循环
            console.log('updated:数据已经更改完成，dom也重新render完成')
        },
        beforeDestroy:function(){
            console.log('beforeDestory:销毁前执行（$destroy方法被调用的时候就会执行）,一般在这里善后:清除计时器、清除非指令绑定的事件等等...')
            // clearInterval(this.$options.timer)
        },
        destroyed:function(){
            console.log('destroyed:组件的数据绑定、监听...都去掉了,只剩下dom空壳，这里也可以善后')
        }
    })


    
    new Vue({
    }).$mount('#app')


</script>
</html>
```
