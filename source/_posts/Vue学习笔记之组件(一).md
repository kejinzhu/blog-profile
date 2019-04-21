---
title: Vue学习笔记之组件(一)
copyright: true
date: 2018-09-05 12:54:23
categories: Vue
tags:
    - Vue学习笔记
    - 组件
---

## 组件

- 组件：把页面中公共部分提炼出来，可以进行复用
- 每一个组件都是一个Vue实例;那么具有生命周期;并且也有data、computed、methods、watch这些属性，每个组件都有自己独立维护的数据;
<!-- more -->

### 组件的注册

#### 全局组件
注册一个组件
组件中data属性值需要是一个函数，并返回一个对象
Vue.component(string,{})

- 1.组件的名字，在JS中名字可以带"-",还可以是驼峰的形式命名
- 2.在HTML中进行引用时，支持"-",但是不支持驼峰
- 3.Vue.component注册的全局组件，可以在其他Vue的实例中使用

```html
<div id="app">
    <hand></hand>
</div>
<div id="app1">
    <hand><hand>
</div>
```

```javascript
Vue.component("hand",{
    data(){
        return {
            msg:'xiaoke'
        }
    },
    template:"<div>{{msg}}</div>"
});
let vm = new Vue({
    el:'#app',
    data:{

    }
});
let vm1 = new Vue({
    el:'#app1',
    data:{

    }
});
```

#### 局部组件

局部组件使用三部曲

- 创建组件
- 注册组件
- 使用组件

1、创建一个组件

每一个组件都是一个Vue的实例;那么每一个组件都有自己的生命周期，都有自己的钩子函数
一个Vue实例就是一个组件

组件中的data是私有的，

在各自的组件中，如果return的对象是一个公有的对象，那么其中一个改变，另一个也会改变

```javascript
let components1 = {
    data(){
        return {
            //data是私有的
            name:"xiaoke"
        }
    },
    methods:{
        fn(){

        }
    },
    template:"<div>{{name}}</div>"
}
```

2、注册组件

```javascript
let vm = new Vue({
    el:'#app',
    data:{            
    },
    components: {
    //注册一些局部组件
        components1
    }
})
```

3、使用组件

```html
<div id="app">
    <components1></components1>
</div>
```

### 组件的嵌套

创建->注册->使用

- 先创建一个组件
- 在对应的父组件中进行注册
- 在父组件的标签中将子组件的标签名放进去

```html
<div id="app">
    <parent></parent>
</div>
```

```javascript
let grandson = {
    data(){
        return {}
    },
    template:"<div>grandson</div>"
};
let son = {
    data(){
        return {}
    },
    template:"<div>son<grandson></grandson></div>",
    components: {
        grandson        
    }
};
let parent = {
    data(){
        return {}
    },
    template:"<div>parent<son></son></div>",
    components: {
        son
    }
};
let vm = new Vue({
    el:'#app',
    data:{

    },
    components: {
        parent    
    }    
})
```

### 组件之间的数据传递

#### 父传子

组件必须至少依托于一个跟组件Vue实例

在子组件中data、computed、watch、methods这些属性都有，因为子组件就是一个Vue的实例

1、把父组件中的数据绑定子组件中的一个行间属性，并且是动态绑定的这个属性
2、利用子组件中的props，去接收一下这个行间的属性
3、在子组件中进行使用

```html
<div id="app">
    <son :m="msg"></son>
</div>
```

```javascript
let son = {
    data(){
        return {
            val:100
        }
    },
    //props可以接受父组件中的数据
    props: ["m"],
    template:"<div>{{val}}{{m}}</div>"
}
let vm = new Vue({
    el:'#app',
    data:{
        msg:'hello'
    },
    components: {
        son
    }
})
```

#### 子传父

如果元素绑定的是一个自定义的事件，需要实例上的$emit方法来触发这个函数

子组件通过$emit触发自定义方法
$emit :存在于每一个Vue的实例上

在Vue中自定义事件需要带有'-'

子传父：如果需要当触发子组件上的操作改变父组件上的数据时，需要在父组件定义方法时，改变自己的值；在父组件中定义的方法，需要订阅到子组件的自定义事件上，当触发子组件中的某个操作时，通过$emit去触发这个自定义的事件，让其对应的父组件的方法执行

```html
<div id="app">
    <div>父亲：{{money}}</div>
    <!-- change是获取到的父组件的methods中的值 -->
    <!-- changemoney自定义事件 -->
    <!-- 如果元素绑定的是一个自定义的事件，需要实例上的$emit方法来触发这个函数 -->
    <son :m="money" @change-money = "change"></son>
    <!-- 语法糖 -->
    <!-- <son :m.sync="money"></son> -->
    <!-- <son v-bind:m="money" v-on:update:m="money=$event"></son> -->
</div>
```

```javascript
let vm = new Vue({
        el:'#app',
        data:{
            money:400
        },
        methods: {
          change(val){
              this.money = val;
            }  
        },
        components: {
            son:{
                data () {
                    return {
                            
                    }
                },
                props: ["m"],
                template:"<div>儿子:{{m}}<button @click='fn'>多要钱</button></div>",
                methods: {                        
                    fn(){
                        //$emit存在于每一个Vue的实例上
                        //需要和DOM行间属性保持一致
                        this.$emit("change-money",800);
                        // 语法糖
                        //this.$emit("update:m",888)
                    }
                }
            }
        }
    })
```

### 组件的单向数据流

单向数据流：只能从父组件传递给子组件，不能从子组件传递给父组件;当父组件的数据发生改变之后，那么子组件的数据也会发生改变

在组件的函数或者事件中，那么函数中的this指向了当前组件的实例

通过props传递过来的数据也具有响应式,但是不能再次传递给父组件，不能影响父组件

如果是一个对象或者数组，那么当子组件改变时，也会影响到父组件;是由于是同一个空间地址的原因;

```html
<div id="app">  
    <son :m = "msg"></son>
</div>
```

```javascript
let vm = new Vue({
    el:'#app',
    data:{
        msg:'hello'
    },
    methods: {
        fn(){
            this.msg = "xiaoke"
        }  
    },
    components: {
        son:{
            data () {
                return {
                
                }
            },
            //props:会把这个属性放到当前的组件的实例的属性上;
            props: ["m"],
            template:"<div @click='fn'>{{m}}</div>",
            methods: {
                fn(){
                    //在组件的函数或者事件中，那么函数中的this指向了当前组件的实例
                    //通过props传递过来的数据也具有响应式,但是不能再次传递给父组件，不能影响父组件
                    console.log(this);
                    console.log(this.m);
                }
            }
        }
    }
})
```

### props验证

props：不仅可以传数组，还可以传对象

```javascript

props: {
    //m:Number单个类型校验
    //m:[Number.String,Object,Function]多个类型校验
    m:{
        type:[],//校验类型的
        required:true,//必填项
        default:100,//默认值
        validator(val){
            //这个函数默认执行
            // console.log(val);
            //会根据当前validator的返回值，向浏览器的控制台抛出警告
            return ['success','warn','danger'].indexOf(val)===-1;
        }
    }
},
```

```html
<div id="app">
    <child :m="msg"></child>
</div>
```

```javascript
let vm = new Vue({
    el: '#app',
    data: {
        msg: 100
    }, 
    components: {
        child: {
            data() {
                return {

                }
            },
            props: {
                //m:Number单个类型校验
                //m:[Number.String,Object,Function]多个类型校验
                m: {
                    type: [],//校验类型的
                    required: true,//必填项
                    default: 100,//默认值
                    validator(val){
                        //这个函数默认执行
                        // console.log(val);
                        //会根据当前validator的返回值，向浏览器的控制台抛出警告
                        return ['success','warn','danger'].indexOf(val)===-1;
                    }
                }
            },
            template: "<div @click='fn'>{{m}}</div>",
            methods: {
                fn() {
                    console.log(typeof this.m);
                }
            }
        }
    }
})
```


