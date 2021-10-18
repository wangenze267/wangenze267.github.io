---
title: 框架简介之Vue.js
author: Ned
tags: Vue
abbrlink: 2624959054
---

## 前端框架有哪些？

### React（GitHub No.1）

React起源于Facebook的内部消息。React官方是这样介绍的它：一个声明式、高效、灵活的、创建用户界面的JavaScript库，即使React的主要作用是构建UI，但是项目的逐渐成长已经使得react成为前后端通吃的WebApp解决方案。

### Vue（GitHub No.2）

vue官网说：Vue.js 是一套构建用户界面的渐进式框架。与其他重量级框架不同的是，Vue 采用自底向上增量开发的设计。
渐进式，我个人理解就是阶梯式向前。vue是轻量级的，它有很多独立的功能或库，我们会根据我们的项目来选用vue的一些功能。就像我们开发项目时如果只用到vue的声明式渲染，我就只用vue的声明渲染，而我们要用他的组件系统，我们可以引用它的组件系统。
vue的渐进式表现为：

<!-- more -->

> 声明式渲染——组件系统——客户端路由——-大数据状态管理——-构建工具

### Augular（GitHub No.3）

AngularJS 是一款开源JavaScript库，由Google维护，用来协助单一页面应用程序运行,它是基于ES6来开发的。

它是前端/客户端JavaScript框架，由Google创建和维护，用于构建功能强大的单页应用程序，通过表达式绑定数据到 HTML。

## Vue.js

### Vue的核心点

#### 1.响应式数据绑定

当数据发生变化时，Vue自动更新视图。

原理是利用了 Object.definedProperty中的setter/getter代理数据，监控对数据的操作。

```vue

<template>
  <div id="app">
    {{ message }}
  </div>
</template>
 
<script>
export default {
  name: 'app',
  data () {
    return {
      message: 'Welcome to Your Vue.js App'
    }
  }
}
</script>
 
<style>
</style>
```

#### 2.可组合的视图组件

一个页面被映射成组件树，将组件进行划分，方便维护，复用，测试，也就是说一个页面是由多个组件组合而成。

![img](https://wangez.site/img/shangke/zujian.png)

![img](https://wangez.site/img/shangke/dom.png)

#### 3.虚拟Dom

虚拟Dom是随着时代发展而诞生的产物。

在Web早期，页面的交互都十分简单，没有复杂的状态需要管理，也不需要频繁操作Dom，使用JQ来开发就能满足我们的需求。

随着时代不断发展，功能越来越多，我们需要实现的需求就越来越复杂，程序中需要维护的状态也就越来越多，导致Dom操作越来越频繁。

那么，我们再像以前那样，使用JQ来发开页面的时候，就会有一大部分代码是在操作Dom，程序中的状态也很难管理，代码的逻辑就会十分混乱。

这其实是命令式操作Dom的问题，虽然简单，但是不好维护。

现在，主流的前端的框架，都是声明式操作Dom。我们通过描述状态和Dom之间的映射关系是怎样的，就可以将状态渲染成视图，至于怎么渲染的，框架会帮我们去做，不用我们手动去操作Dom。

运行的JavaScript代码速度是很快的，但是有大量的操作DOM就会很慢，时常在更新数据后会重新渲染页面，这样造成在没有改变数据的地方也重新渲染了DOM节点，这样就造成了很大程度上的资源浪费。

**利用在内存中生成与真实DOM与之对应的数据结构，这个在内存中生成的结构称之为虚拟DOM**

当数据发生变化时，能够智能地计算出重新渲染组件的最小代价并应用到DOM操作上

![img](https://wangez.site/img/shangke/xunidom1.png)

![img](https://wangez.site/img/shangke/xunidom2.png)

#### 4.MVVM

**MVVM概述：**M：Model数据模型 ， V：view 视图模板  ， VM：view-Model：视图模型

![img](https://wangez.site/img/shangke/mvvm.png)

Vue中的MVVM实例（双向数据绑定）：当输入框输入数据的时候，相应的message也会改变

```
<template>
  <div id="app">
    <input type="text" v-model="message"/>
    {{ message }}
  </div>
</template>
 
<script>
  export default {
    name: 'app',
    data () {
      return {
        message: 'Welcome'
      }
    }
  }
</script>
 
<style>
</style>
```

#### 5.声明式渲染

Vue.js 的核心是一个允许采用简洁的模板语法来声明式的将数据渲染进 DOM，初始化根实例，vue自动将数据绑定在DOM模板上

声明式渲染与命令式渲染区别

声明式渲染：所谓声明式渲染只需要声明在哪里，做什么，而无需关心如何实现

命令式渲染：需要具体代码表达在哪里，做什么，如何实践
需求：求数组中每一项的倍数，放在另一个数组中

**命令式渲染：**

```
var arr = [1, 2, 3, 4, 5]
  var newArr = []
  for (var i = 0; i < arr.length; i++) {
    newArr.push(arr[i] * 2)
  }
  console.log(newArr)
```

**声明式渲染：**

```
var arr = [1, 2, 3, 4, 5]
  var newArr = arr.map(function (item) {
    return item * 2
  })
  console.log(newArr)
```

