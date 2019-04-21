---
title: react学习笔记(十六)
copyright: true
date: 2018-09-08 09:22:00
categories: redux
tags:
    - redux
    - redux的工程化结构
---

## redux工程化目录

 [文件目录]
 store:
    reducer:存放每一个模块的reducer
      vote.js
      personal.js
      ...
      index.js:把每一个模块的reducer最后合并成为一个reducer
    action:存放每一个模块需要进行的派发任务(ActionCreator)
      vote.js:
      personal.js
      ...
      index.js:所有模块的Action进行合并
  action-types.js所有派发任务的行为标识都在这里进行宏观管理
  index.js:创建store

### action-types.js

```javascript
/**
 * 管控当前项目中所有redux任务派发中需要的行为标识ACTION-TYPE
 * 
 */

//=>VOTE 

export const VOTE_SUPPORT = 'VOTE_SUPPORT';
export const VOTE_AGAINST = 'VOTE_AGAINST';

//=>PERSONAL

export const PERSONAL_INIT = 'PERSONAL_INIT';


```

### reducer/personal.js

```javascript
import * as TYPE from "../action-types";
export default function personal(state = {
    baseInfo:{}
},action) {
    return state;
}
```

### 