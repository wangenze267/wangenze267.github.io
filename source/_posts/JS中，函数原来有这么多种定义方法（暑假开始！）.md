---
title: JS中，函数原来有这么多种定义方法（暑假开始！）
author: Ned
tags:
  - JavaScript
  - ES6
categories:
 - JavaScript基础
abbrlink: 1173819435
---

### 前言

最近在期末与暑假假期画饼之间不断奔波，很长时间没有记录了，今天在学习的时候看到了ES6的箭头函数，瞬间找到了写文的动力，那就来梳理一下在JavaScript中一共有几种定义函数的方式吧，最后再（微微）重点的介绍一下箭头函数✨

过段时间，要加强算法学习了，这段时间也在力扣和牛客上找题做了一下，发现自己的算法真的烂，所以可能以后还会把算法题解写一写。（🍗+ 1）

### JavaScript定义函数的几种方式

以下是定义函数的几种方式，真正书写的话，个人觉得没有什么必须要用某种格式的情况，根据团队风格或者个人习惯来就好。

<!-- more -->

#### 最初的我们

我们刚接触js的时候，见到的就是它啦😎

```javascript
function hello () {
	console.log('hello javascript');
}
hello();
```

这种方式大概也是陪伴我们时间最长的了吧？是不是呢？

#### 函数表达式

函数都是Function的实例对象，也就可以说函数是值，与字符串，数字等是一样的。这种方法就是通过表达式来定义函数，也就是定义匿名函数，再将匿名函数交给变量。

```javascript
var hello = function () {
	console.log('hello javascript');
}
hello();
```

#### 对象方法

这种方法大家应该也都见过，它是定义匿名函数来作为对象属性的值，与函数表达式的理念相同。这种形式最早是在ES3中引入的。

```javascript
let obj = {
	fun: function() {},
}
```

ES5中，引入了访问器属性定义:

```javascript
let obj = {
	get name() {},
	set name(name) {}
}
```

看到get/set是不是感觉跟Java的类有一点像了，没错我也有这种感觉，继续往下看，类它来了！

#### 类

在ES6中，有了类的概念（学过java的小伙伴们一定不会陌生），没错，js中也有类，而且也有构造方法等一些与Java类一样的东西。

```javascript
class Demo{
	constructor(){
        this.fun=()=>{
            console.log('实例方法')
        }
		console.log('构造方法');
	}
	static jingtai(){
		console.log('静态方法');
	}
	yuanxingfangfa(){
		console.log('原型方法')
	}
}
```

- 实例方法：定义在构造方法constructor中的this对象上，在类中，构造方法中的this指的是它的实例对象
- 原型方法：与constructor方法同级，可以通过构造函数的原型链去调用，也可以通过实例对象来调用
- 静态方法：通过static关键字定义，不能被实例调用，可以被构造函数调用。

#### 箭头函数

终于到了我们这次文章的主角登场啦~  我去网上查了一些资料，浏览了CSDN和掘金的一些博客社区，很多人都觉得，它是ES2015最具有争议性的函数之一，但是它如今也已经变得众所周知，无处不在。它为函数声明提供了两种不同的格式，赋值表达式（箭头后无大括号"{}"）和函数体（代码种包括0到多个语句时）。这个语法还允许在描述单个参数的时候不加圆括号，0个或者1个以上的时候要加圆括号。这些语法结构允许箭头函数有着多种书写格式。

```javascript
// 无参的赋值表达式
(() => 2*2);

// 一个参数，忽略括号的赋值表达式
(x => x*2);
// 一个参数，忽略括号直接跟函数体
(x => { return x *2 });

// 括号中为参数列表和赋值表达式
((x,y) => x*y);
```

来稍微解释一下箭头函数

```javascript
x => x * 2;
```

等价于：

```javascript
function (x) {
	return x * 2;
}
```

箭头函数相当于匿名函数，并且简化了函数定义，使用起来非常的方便。

但是使用箭头函数，我们要注意以下几点：

- 箭头函数是没有this的，它的this是从外部获取的。

- 箭头函数不能做new操作，没有this意味着它不能用作构造，所以不能用new调用它们

  ```javascript
  var jiantou = () => {};
  var fn = new jiantou();
  ```

  这时候会报错，`jiantou is not a constructor`

- 箭头函数没有arguments对象，但是同this一样，他可以访问外围函数的arguments对象

- 箭头函数没有原型和super，它没有原型，也就不能通过super来访问原型的属性，不过同之前的this、arguments一样，这些值由外围最近一层的非箭头函数决定

  ```javascript
  var jiantou = () => {};
  console.log(jiantou.prototype); //undefined
  ```

### 最后

🐱‍🐉终于水完了，溜了溜了。

还是那句话，你可以暂时不使用这种方法来写代码，但是你不能不会用，因为你不会知道什么时候会突然要求你用这种方法去进行业务操作。

快乐的暑假，开始了！