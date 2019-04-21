---
title: ECharts使用过程遇到的问题汇总
copyright: true
date: 2018-08-18 11:18:30
categories: 前端 ECharts
tags: 
    - ECharts
---

# 获取ECharts
> npm install echarts --save
<!-- more -->
# 自定义构建ECharts
我选用的是常用版的echarts/dist/echarts.common.js
在我的项目根目录下myProject新建echarts.common.js，然后将echarts.common.js压缩到lib下的压缩文件echarts.common.min.js时遇到一点问题
首先是各种包没有安装，就依次安装就好
其次是出现如下错误：
> TypeError: uglifyPlugin is not a function

我采用的是安装webpack，可能需要打包工具，然后是rollup-plugin-uglify版本太高，重新安装rollup-plugin-uglify@3.0.0，再重新安装就可以了

