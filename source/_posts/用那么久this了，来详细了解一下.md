---
title: 用那么久this了，来详细了解一下
author: Ned
tags:
  - JavaScript
abbrlink: 724479206
date: 2021-10-06 08:00:15
---

## 前言
this想必大家都很不陌生了，在例如`Vue.js`中，各种this，唰唰唰的写，但是有没有遇到this指向出错的问题呢？

我有，我猜应该也会有人跟我一样。

所以，我总结了一些this的基础概念和基本使用在这里，供大家参考。

<!--more-->

## this出现在哪里

**全局上下文中的this**

console.log(this)来打印出来全局执行上下文中的 this，最终输出的是 window 对象。

所以可以得出这样一个结论：全局执行上下文中的 this 是指向 window 对象的。这也是 this 和作用域链的唯一交点，作用域链的最底端包含了 window 对象，全局执行上下文中的 this 也是指向 window 对象

**函数上下文中的this**

在全局环境中调用一个函数，函数内部的 this 指向的是全局变量 window。

通过一个对象来调用其内部的一个方法，该方法的执行上下文中的 this 指向对象本身

```javascript
function foo(){
	console.log(this)
};
foo(); // window

let a = {
	b:0,
	fn:function(){
		console.log(this)
	}
}
a.fn(); //{ b:0, fn:f() }
```

## this指向总结

* 当函数被正常调用时，在严格模式下，this 值是 undefined，非严格模式下 this 指向的是全局对象 window；

* 通过一个对象来调用其内部的一个方法，该方法的执行上下文中的 this 指向对象本身

* ES6 中的箭头函数并不会创建其自身的执行上下文，所以箭头函数中的 this 取决于它的外部函数

* new 关键字构建好了一个新对象，并且构造函数中的 this 其实就是新对象本身

* 嵌套函数中的 this 不会继承外层函数的 this 值。

  ```javascript
  var myObj = { 
  	name : "Ned", 
  	showThis: function(){ 
  		console.log(this); // myObj
          var bar = function(){ 
          	this.name = "阿泽"; 
          	console.log(this) // window
          } 
          bar(); 
      }
  };
  myObj.showThis();
  console.log(myObj.name); // Ned
  console.log(window.name); // 阿泽
  ```

  * 解决this不继承的方法

    * 内部函数使用箭头函数
    * 将在外层函数中创建一个变量，用来存储this，内层函数通过作用域链即可访问

    ```javascript
    var myObj = { 
    	name : "Ned", 
    	showThis:function(){ 
    		console.log(this); // myObj
            var bar = ()=>{ 
            	this.name = "阿泽"; 
            	console.log(this) // window
            } 
            bar(); 
        }
    };
    myObj.showThis();
    console.log(myObj.name); // 阿泽
    console.log(window.name); //  
    ```

    ```javascript
    var myObj = { 
    	name : "Ned", 
    	showThis:function(){ 
    		console.log(this); // myObj
            var self = this;
            var bar = function (){ 
            	self.name = "阿泽"; 
            	console.log(self) // window
            } 
            bar(); 
        }
    };
    myObj.showThis();
    console.log(myObj.name); // 阿泽
    console.log(window.name); // 
    ```

    

## 改变this指向的方法

### call 和 apply 的共同点

都能够**改变函数执行时的上下文**，将一个对象的方法交给另一个对象来执行，并且是立即执行的

> **调用 call 和 apply 的对象，必须是一个函数 Function**

### call 和 apply 的区别

#### call 的写法

```javascript
Function.call(obj,param1,param2,...)
```

需要注意以下几点：

- 调用 call 的对象，必须是个函数 Function。
- call 的第一个参数，是一个对象。 Function 的调用者，将会指向这个对象。如果不传，则默认为全局对象 window。
- 第二个参数开始，可以接收任意个参数。每个参数会映射到相应位置的 Function 的参数上。但是如果将所有的参数作为数组传入，它们会作为一个整体映射到 Function 对应的第一个参数上，之后参数都为空。

```javascript
function func (a,b,c) {}

func.call(obj, 1,2,3)
// func 接收到的参数实际上是 1,2,3

func.call(obj, [1,2,3])
// func 接收到的参数实际上是 [1,2,3],undefined,undefined
// 其实func还是接收了三个参数，只不过咱们只传了一个过去，这个应该很容易理解的
```

#### apply 的写法

```
Function.apply(obj[,argArray])
```

**需要注意的是：**

- 它的调用者必须是函数 Function，并且只接收两个参数，第一个参数的规则与 call 一致。
- 第二个参数，必须是数组或者类数组，它们会被转换成类数组，传入 Function 中，并且会被映射到 Function 对应的参数上。这也是 call 和 apply 之间，很重要的一个区别。

```
func.apply(obj, [1,2,3])
// func 接收到的参数实际上是 1,2,3

func.apply(obj, {
    0: 1,
    1: 2,
    2: 3,
    length: 3
})
// func 接收到的参数实际上是 1,2,3
```

### call 和 apply 的用途

下面会分别列举 call 和 apply 的一些使用场景。
>声明：例子中没有哪个场景是必须用 call 或者必须用 apply 的，看个人习惯就好。

#### call 的使用场景

**对象的继承**如下面这个例子：

```javascript
function superClass () {
    this.a = 1;
    this.print = function () {
        console.log(this.a);
    }
}

function subClass () {
    superClass.call(this);  // 执行superClass，并将superClass方法中的this指向subClass
    this.print();
}

subClass();
// 1
```

subClass 通过 call 方法，继承了 superClass 的 print 方法和 a 变量。

此外，subClass 还可以扩展自己的其他方法。

###  bind

#### bind 的用法

在 MDN 上的解释是：bind() 方法创建一个新的函数，在调用时设置 this 关键字为提供的值。并在调用新函数时，将给定参数列表作为原函数的参数序列的前若干项。

它的语法如下：

```
Function.bind(thisArg[, arg1[, arg2[, ...]]])
```

bind 方法 与 apply 和 call 比较类似，也能改变函数体内的 this 指向。

不同的是，**bind 方法的返回值是函数，并且需要稍后调用，才会执行**。

而 apply 和 call 则是立即调用，来看下面这个例子：

```
function add (c) {
    return this.a + this.b + c;
}

var obj = {a:1,b:2}

add.bind(obj, 5); // 这时，并不会返回 8
add.bind(sub, 5)(); // 调用后，返回 8
```

如果 bind 的第一个参数是 null 或者 undefined，this 就指向全局对象 window。

## 最后
"abc"的使用，具体还是要看个人运用，理解了之后，那它们就变成了工具，怎么顺手怎么来了~

顺便说一下本人，我还是喜欢apply多一点hhh，用它的次数多一点，所以在场景用谁都行的时候，我一般都会选择apply。
