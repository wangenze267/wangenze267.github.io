---
title: 什么？你还不知道Symbol？
tags:
  - JavaScript
  - ES6
abbrlink: 2670144936
date: 2021-09-25 13:25:19
---

### 前言

ES6引入了一种新的原始数据类型`Symbol`表示独一无二的值。它是JavaScript语言的第七种数据类型，是一种类似于字符串的数据类型。
### Symbol的特点
- Symbol的值是唯一的，用来解决命名冲突的问题
- Symbol值不能与其他数据进行运算
- Symbol定义的对象属性不能使用`for...in`循环遍历，但是可以使用`Reflect.ownKeys`来获取对象的所有键名

<!--more-->

### 创建Symbol的两种方式
#### 创建Symbol
```js
    // 创建Symbol
    let s = Symbol();
    console.log(s, typeof s); // Symbol() 'symbol'
```
打印出来的数据跟想象中是不是不一样？是不是觉得`s`应该是一个很长的东西？这是因为它的唯一性在这里是不可见的，所以我们才看不见。

不要慌，继续往下看，我们继续创建Symbol。
```js
    let s2 = Symbol('Ned');
    let s3 = Symbol('Ned');
    console.log(s2 === s3); // false
```
我们现在传入了两个字符串，想看看返回的值是不是一样的，发现是不一样的。
> 我的理解：
> 虽然传入两个字符串内容一样，但是他们的编号未必一样，啊不，应该是一定不一样。
#### 使用Symbol.for创建Symbol
这样创建的Symbol是一个对象。
```js
     let s4 = Symbol.for('Ned');
     console.log(s4, typeof s4); // Symbol(Ned) 'symbol'
```
通过`Symbol.for`创建的，我们可以通过传入同一字符串来得到唯一的symbol值的。
```js
    let s4 = Symbol.for('Ned');
    let s5 = Symbol.for('Ned');
    console.log(s4 === s5); // true
```
#### 【强调】Symbol值不能与其他数据进行运算
```js
    let a = Symbol();
    let b = a + 100;
    let c = a + '100';
```
这都是不行的，会直接在浏览器中报错。
### Symbol的基本使用
> symbol的使用场景就是给对象加属性或者方法
```js
    let game = {
        name: '石头剪刀布'，
        ...
        up: function() {
            console.log('这是up函数');
        }，
        down: function() {
            console.log('这是down函数');
        }
        ...
    }
```

现在有一个对象`game`,我们想往里添加两个方法，分别是`up`和`down`，但是我们不清楚现在对象中是否已经具有`up`和`down`两个方法，按照以往的方法，我们是不是应该打开这个对象，去里面一一查找是否具有这两个方法，如果找到了，我们是不是就应该换个名字，如果没找到，我们就可以向内添加方法，但是这个对象结构简单还好，如果稍微复杂一点的话，这个操作就会非常复杂，会耗费许多时间。

在这种应用场景下，我们就应该应用`Symbol`来创建唯一值，完成这个需求。

```js
    // 声明一个method对象
    let methods = {
        up: Symbol(),
        down: Symbol()
    }
    // 向game对象中，注入方法
    game[methods.up] = function(){
        console.log('我要上升！');
    }
    game[methods.down] = function(){
        console.log('我要下降！');
    }
```
我们使用这样的方式，给`game`对象添加方法，是不会破坏`game`原有的一些属性的，是非常安全快速的。

下面我们打印一下`game`对象，看一下他内部是什么样子的。
```js
    name: "石头剪刀布"
    down: f()
    up: f()
    Symbol(): f()
    Symbol(): f()
```
可以看到，多出了两个Symbol函数，没有对原函数造成影响。

### 数据类型小诀窍
送大家一个记住JavaScript数据类型的小诀窍

共八种

> **USONB: you are so niu bility** 
>
> **U: undefined**
>
> **S: string  symbol**
>
> **O: object**
>
> **N: null  number**
>
> **B: boolean  bigint**

> **想顺便复习数据类型的话也可以看我的这篇文章：[一文带你了解JavaScript的数据类型](https://blog.wangez.site/posts/2884264051.html/#more)**
### 最后
希望总结的这些知识可以让大家对symbol类型的数据有一个简单的了解。
