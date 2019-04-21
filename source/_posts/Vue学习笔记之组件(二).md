---
title: Vue学习笔记之组件(二)
copyright: true
date: 2018-09-05 18:20:34
categories: Vue
tags:
    - Vue学习笔记
    - 组件
---

## 父子组件的mounted

父子组件会先执行谁的mounted？

如果有父子组件，会先触发子组件的mounted，然后再触发父组件的mounted，为了在父组件中更方便的操作DOM元素
<!-- more -->

```html
<div id="app">
   <!-- 父子组件会先执行谁的mounted？-->
    <!--如果有父子组件，会先触发子组件的mounted，然后在触发父组件的mounted；为了在父组件中放方便操作DOM元素；-->
    <child></child>
</div>

<template id="divs"  >
    <div>
        <ul ref="a">
            <li v-for="a in  arr"></li>
        </ul>
    </div>
</template>
```

```javascript
let  child = {
    template:"#divs",
    data(){
        return {
            arr:[1,2,3]
        }
    },
   /* created(){
        console.log(300);
    },*/
    mounted(){
        this.arr = [1,2,3,4,5];
        // DOM的渲染是异步的；
        console.log(100);
    }
}
let vm = new Vue({
    el: '#app',
    data: {},
    /*created(){
        console.log(400);
    },*/
    mounted(){
        console.log(200);
        // $nextTick : 会等待异步执行完成，才会执行；
        this.$nextTick(()=>{
            // 会等待当前组件的DOM，子组件的DOM渲染完成，才可以执行；
            console.log(this.$children[0].$refs.a.children.length);
        })
    },
    components:{
        child
    }
});
```

## 动态组件

在使用component这个自定义组件时，需要配合:is这个属性，动态绑定一个组件的名称

is后面的属性值是哪个组件的名称，显示哪个组件；不可以同时显示两个

```html
<div id="app">
    <input type="radio" v-model="title" value="home">home
    <input type="radio" v-model="title" value="list">list
    <!-- 在使用component这个自定义组件时，需要配合:is这个属性，动态绑定一个组件的名称；-->
    <!--is后面的属性值是哪个组件的名称，显示哪个组件；不可以同时显示两个-->
    <keep-alive>
        <!---->
        <component v-bind:is="title"></component>
    </keep-alive>

    <!--<component is="home"></component>-->

</div>
```

```javascript
// 创建组件
// 动态组件进行切换时，会将上一个组件进行销毁，然后再挂载最新的组件；
//keep-alive: 保存了组件第一次生成时的状态；以后需要该组件，直接读取缓存中的组件就可以；不再执行挂载，销毁；从而提高了浏览器的性能；
let home  = {
    template:"<div>home</div>",
    mounted(){
        console.log("挂载home")
    },
    destroyed(){
        console.log("已销毁home")
    }
};
let list  = {
    template:"<div>list</div>",
    mounted(){
        console.log("挂载list")
    },
    destroyed(){
        console.log("已销毁list")
    }
};
let vm = new Vue({
    el: '#app',
    data: {
        title:"home",

    },
    // 注册组件
    components:{
        home,list
    }
});
```

## 事件车eventBus

每一个Vue的实例都是独立的，相互之间是不能直接进行改变数据的

给两个不同的组件找一个载体;把共同的方法放在这个载体上

```javascript
let eventBus = new Vue;//创建一个新的Vue实例
```

在这个新的实例上,有$on:订阅  $emit:发布

```html
<div id="app">
    <brother1></brother1>
    <brother2></brother2>
</div>
```

```javascript
// 每一个Vue的实例都是独立的，相互之间是不能直接进行改变数据的
// 给两个不同的组件找一个载体;把共同的方法放在这个载体上
let eventBus = new Vue;//创建一个新的Vue实例
// 在这个新的实例上,有$on:订阅  $emit:发布
let brother1 = {
    data () {
        return {
            color:"绿色"
        }
    },
    created () {
        // 给事件车订阅changeGreen方法
        // $on：param1：自定义事件名称 ,param2：方法名称
        eventBus.$on('changeG',this.changeGreen);  
    },
    methods: {
        changeGreen(){
            this.color = "变红";
        },
        green(){
            eventBus.$emit("changeR");
        }  
    },
    template:"<div>{{color}}<button @click='green'>变绿</button></div>"
}
let brother2 = {
    data () {
        return {
            color:"红色"
        }
    },
    created () {
    // 给事件车订阅changeRed方法
        eventBus.$on('changeR',this.changeRed);  
    },
    methods: {
        changeRed(){
            this.color = "绿色";
        },
        red(){
            eventBus.$emit("changeG");
        }
    },
    template:"<div>{{color}}<button @click='red'>变红</button></div>"
}
let vm = new Vue({
    el:'#app',
    data:{

    },
    components: {
        brother1,
        brother2
    }
})
```