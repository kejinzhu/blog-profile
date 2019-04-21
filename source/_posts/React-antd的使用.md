---
title: '[React]-antd的使用'
copyright: true
date: 2018-10-04 16:23:06
categories: react
tags:
    - react
    - antd
---
## antd的使用步骤
### 1、安装

> yarn add antd

### 2、快速使用
- 1、导入antd：需要使用哪些组件,就导入哪些组件
- 2、导入antd的样式：我们自己建立一个CSS样式表,在样式表中基于@import导入antd.css
- 3、antd提供的组件都是英文国际化的,需要中文显示,我们导入汉化模块

```javascript
import React,{Component} from 'react';
import ReactDOM from 'react-dom';
import './index.css';

//antd的应用
import {DatePicker,Icon,Button,localeProvider,Calendar} from 'antd';
import zh_CN from 'antd/lib/locale-provider/zh_CN';
import 'moment/src/locale/zh_cn'
class A extends Component{
    constructor(props,context){
        super(props,context)
    }
    render(){
        //只要localeProvider包含的组件都是被汉化的
        return (<LocaleProvider locale={zh_CN}>
            <div>
                <DatePicker/>
                <Calendar/>
                <Icon type="zhihu" style={{
                    color:'red',
                    fontSize:'22px'
                }}/>
            </div>
        </LocaleProvider>)
    }
}
```

CSS文件

```css
@import "~antd/dist/antd.css";/*导入antd的样式*/

```