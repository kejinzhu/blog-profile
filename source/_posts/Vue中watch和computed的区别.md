---
title: Vue中watch和computed的区别
copyright: true
date: 2018-09-03 15:11:45
categories: Vue
tags:
    - Vue学习笔记
    - computed和watch的区别
---
# computed和watch的区别
## 从属性名上分析

- computed是一个计算属性，也就是依赖其他的属性计算所得出最后的值
- watch是去监听一个值的变化，然后执行相对应的函数。
<!-- more -->
## 从实现上分析：

- computed的值再getter执行后是会缓存的，知识在它依赖的属性值改变之后，下一次获取computed的值才会重新调用对应的getter来计算;支持缓存
- watch在每次监听的值变化时，都会执行回调。不支持缓存。

## 从场景上分析：

- 如果一个值依赖多个属性(多对一),用computed肯定是更加方便的
- 如果一个值变化后会引起一系列操作，或者一个值变化会引起一系列的变化(用watch会更加方便一些)

## 从计算上分析

- computed通常就是简单的计算
- watch的回调里面会传入监听属性的新旧值,通过这两个值可以做一些特定的操作。

## 从数据上分析：

- computed是用于定义基于数据之上的数据
- watch是你想在某个数据变化时做一些事情

注：如果watch是你想在某个数据变化时做的事情是更新其他数据，那其实与把这个要更新的数据项定义成computed是一样的，这个时候用computed更有可读性,虽然在技术上讲watch也可以实现。

## 从写法上分析：

- computed的返回是state处理后的结果
- watch是赋值行为，修改state

## 从异步上分析：

- computed不支持异步
- watch支持异步