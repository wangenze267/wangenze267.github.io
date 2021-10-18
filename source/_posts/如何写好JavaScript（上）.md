---
title: 如何写好JavaScript（上）
author: Ned
tag:
  - 字节青训营
  - JavaScript
abbrlink: 98016397
---

### 前言

课程记录笔记第三弹------《跟月影学JavaScript》

> 今天真被月影大佬讲的js干的有点自闭了，1小时50分钟的视频，我楞是花了将近3个多小时去理解，还一脸懵。
>
> 

本次课题：**如何写好JavaScript（上）**

> 估计，如何写好JavaScript（下）我要后半夜更了，歇一会歇一会

<!--more-->

### 如何写好JavaScript（上）

**写好JavaScript的一些原则**

- 各司其责
- 组件封装
- 过程抽象

#### 写一个🌰

写一段js，控制一个网页，让他支持浅色和深色两种浏览模式。

如果是你来实现，你会怎么做？

```javascript
const btn = document.getElementById('modeBtn');
btn.addEventListener('click',(e)=>{
    const body = document.body;
    if(e.target.innerHTML === '☀'){
        body.style.backgroundColor = 'black';
        body.style.color = 'white';
        e.target.innerHTML = '🌙';
    }else{
        body.style.backgroundColor = 'white';
        body.style.color = 'black';
        e.target.innerHTML = '☀';
    }
});
```

这是通过页面的一个`button`来改变的，这个代码也是最最最简单的一个例子。

> 当然，这个代码是可以优化的，因为现在不便于进行维护

```javascript
const btn = document.getElementById('modeBtn');
btn.addEventListener('click',(e)=>{
    const body = document.body;
    if(body.className !== 'night'){
        body.className = 'night';
    }else{
        body.className = '';
    }
});
```

版本二，它来了。

它将版本一js操作的一些html样式放到两个类中，最后通过js来改变他的类名做到绑定与解除样式。

甚至，只用css也可以实现交互，版本三，他也来了。

> 这是我从未有过的思路，在今天第一次见到

```css
#modeCheckBox:{
    display:none;
}
#modeCheckBox:checked+ .content{
	background-color: black;
	color: white;
	transition: all 1s;
}
```

利用一个`checkbox`，的checked事件，首先将他隐藏起来，再用一个`lable`绑定上他，之后点击`lable`就会触发checked事件，利用选择器完成这个简单的交互

> 这个确实是我没想到的，拿出小本本，圈起来

**小笔记（总结）**

- html/css/js各司其责
- 应当避免不必要的由js直接操作样式
- 可以用class来表示状态
- 纯展示类交互寻求零js解决方案

### 组件

组件是指Web页面上抽出来的一个个包含模板（HTML）、功能（js）和样式（css）的单元。好的组件具备封装性，正确性，扩展性，复用性。

**用原生js写一个轮播图**

- 结构---HTML：
  - 轮播图是一个典型的列表结构，我们可以使用无序列表`<ul>`元素来实现。
- 表现---CSS：
  - 使用css绝对定位将图片重叠在同一个位置
  - 轮播图切换的状态使用修饰符（modifier）
  - 轮播图的切换动画使用css transition
- 行为---JS
  - API的设计应保证原子操作，职责单一，满足灵活性

- 行为---控制流
  - 使用自定义事件来解耦

**总结：基本方法**

- 结构设计
- 展现效果
- 行为设计
  - API（功能）
  - Event（控制流）

#### **重构：插件化**

解耦：

- 将控制元素抽取成插件
- 插件与组件之间通过**依赖注入**方式建立联系

#### **重构：模板化**

解耦：

- 将html模板化，更易于扩展

> 在插件中，抽象出来一个方法去渲染html

#### **重构：组件框架**

抽象：

- 将通用的组件模型抽象出来

#### **总结：组件封装**

- 组件设计的原则：封装性、正确性、扩展性、复用性
- 实现组件的步骤：结构设计、展现效果、行为设计
- 三次重构：
  - 插件化
  - 模板化
  - 抽象化（组件框架）

### **过程抽象**

- 用来处理局部细节控制的一些方法
- 函数式编程思想的基础应用

> function(x){
>
> ​	处理x;
>
> ​	return x;
>
> }

#### 写一个🌰：

一个列表（todolist），完成后点击会消失掉

操作次数限制：

- 一些异步交互
- 一次性的HTTP请求

```javascript
const list = document.querySelector('ul');
const buttons = list.querySelectorAll('button');
buttons.forEach((button)=>{
    button.addEventListener('click',(evt)=>{
        const target = evt.target;
        target.parentNode.className = 'completed';
        setTimeout(()=>{
            list.removeChild(target.parentNode);
        },2000);
    });
});
```

这个代码是可以实现的，但是却有bug，你连续点击按钮的话，他会调用多次`addEventListener`，最后会报错，因为此时节点已经被remove了。

#### **Once**

- 为了能够让“只执行一次”的需求覆盖不同的事件处理，我们可以将这个需求剥离出来。这个过程我们称为**过程抽象**

```js
    function once(fn){
        return function (...args){
            if(fn){
                const ret = fn.apply(this,args);
                fn = null;
                return ret;
            }
        };
    }
    button.addEventListener('click',once((evt)=>{
        const target = evt.target;
        target.parentNode.className = 'completed';
        setTimeout(()=>{
            list.removeChild(target.parentNode);
        },2000);
    }));
```

我们加一个once函数，以fn函数作为参数，第一次进来会调用this指针执行这个函数，执行过后会将fn=null，这样再次点击的时候就不会再次调用了。

#### **高阶函数**

- 以函数作为参数
- 以函数作为返回值
- 常用于作为函数装饰器

#### **常用高阶函数**

- Once
- Throttle  节流函数
- Debounce  防抖函数
- Consumer / 2  异步消耗
- Iterative  迭代函数

#### **思考**

为什么要用高阶函数？

### **编程范式**

命令式与声明式

- 命令式

  强调how，怎么做

- 声明式

  强调what，做什么

#### **编程范式---总结**

- 过程抽象/HOF/装饰器
- 命令式/声明式

### 最后

这次笔记记录的很匆忙，再加上理解的也不是很懂，所以导致后面的高阶函数部分基本没有自己的梳理，后面梳理过后会过来补上更新的。