---
title: 关于promise的使用方法
author: Ned
tags:
  - ES6
  - JavaScript
abbrlink: 3499638177
date: 2021-10-08 17:19:49
---

# Promise

## 前言

好几天前就想写一个promise的笔记了，但是一直以来就仅仅知道它是用来解决回调地狱问题的，没有一个详细的了解，所以在这几天学习的时候，针对它名下的几个方法，做了一个简要的使用介绍。

> promise：这就是我的说明书！
>
> 我：可能说的不是太全，多包涵~

<!--more-->

## 先来了解一下它

什么是promise？它是一个类？一个对象？一个数组？

我们先打印它来看一看吧：

```js
console.dir(Promise);
```

<img src="认识promise.png" style="zoom:60%;" />

打印完了，我们来正式认识一下它。

**promise**是一个构造函数，是ES6提出的异步编程解决方案，用来解决**回调地狱**这种问题，从打印可以看出，它有`reject`、`all`、`resolve`等方法，它的原型上有`catch`、`then`等方法。

**还有一种说法来自于网络：**promise，意为承诺，承诺过一段时间给你结果。promise有三种状态，分别为pending（等待），fulfiled（成功），rejected（失败），状态一旦经过改变，就不会在变。

```js
// fn1执行，如果a>10 执行fn2 ，如果a == 11，执行fn3

function fn1(a, fn2) {
	if (a > 10 && typeof fn2 == 'function') {
	    fn2(a,function(){
	    	if(a == 11){
	    		console.log('this is fn3')
	    	}
	    })
	}
}
fn1(11, function(a, fn3) {
    console.log('this is fn2')
    fn3()
})

```

上面说了promise的提出是用来解决回调地狱的问题，那么什么是回调地狱呢？可以参考一下我这段代码，不断的嵌套回调函数之后，代码就会变得非常繁琐，看代码的时候眼睛不舒服，脑子也不舒服，这种嵌套回调非常多的情况，就叫做**回调地狱**。

## 如何使用promise

```js
var p = new Promise(function(resolve, reject){
    //做一些异步操作
    setTimeout(function(){
        console.log('执行完成');
        resolve('写啥都行');
    }, 3000);
});
```

Promise的构造函数接收一个function，并且这个函数需要传入两个参数：

- resolve ：异步操作执行成功后的回调函数
- reject：异步操作执行失败后的回调函数

### then

还记得上面我写的那个嵌套非常多的例子吗？啊，不记得，那你翻一翻~

Promise的优势就在于，可以在`then`方法中继续写Promise对象并返回，然后继续调用then来进行回调操作。

所以，从表面上看，Promise只是能够简化层层回调的写法，而实质上，Promise的精髓是“状态”，用维护状态、传递状态的方式来使得回调函数能够及时调用，它比传递callback函数要简单、灵活的多。所以使用Promise的正确场景是这样的：

```js
function fn1(a){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
       	resolve(a);
    });
    return p;            
}
fn1(11)
.then(function(a){
	return new Promise(function(resolve, reject){
		if(a > 10){
	   		console.log('a大于10')
   		}
   		resolve(a);
	})
})
.then(function (a){
	if(a == 11){
		console.log('a等于11')
	}
})
```

> 我将上面那个改写了一下。最后那个没有return出来是我后面没有继续then了。

这其实就是链式写法。then就相当于我们之前的callback。

then方法中，不光可以return promise对象，也可以return数据：

```js
function fn1(a){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
       	resolve(a);
    });
    return p;            
}
fn1(11)
.then(function(a){
		if(a > 10){
	   		console.log('a大于10')
   		}
   		return a
})
.then(function (a){
	if(a == 11){
		console.log('a等于11')
	}
})
```

### reject

把promise的状态从`pending`改成`rejected`，之后我们就可以在`then`中执行失败情况的回调，来看这个例子：

```js
function fn1(a){
    var p = new Promise(function(resolve, reject){
       	if(a>10){
       		resolve(a);
       	}else {
       		reject(a);
       	}
    });
    return p;            
}
fn1(9)
.then((a) => {
	console.log('a大于10')
},	(err) => {
	console.log('a小于10')
})
```

**then可以接收两个参数，分别对应着resolve的回调和reject的回调，所以在调整传入的a的值，我们可以得到两个结果。**

> 即a大于10和a小于10

### catch

对其他语言有了解的人应该可以知道，catch是用来抓取异常的，那么在promise里，它的作用也一样，**它就如同then的第二个参数，对应着reject的回调**

写法是这样：

```js
.then((a) => {
	console.log('a大于10')
}).catch((err) => {
	console.log('a小于10')
})
```

效果和写在then的第二个参数里面是一样的。

不过它还有另外一个作用：在执行resolve的回调（也就是上面then中的第一个参数）时，如果抛出异常了（代码出错了），那么并不会报错卡死进程，而是会进到这个catch方法中。

> 就很像 try catch 

再来看这段代码

```js
function fn1(a){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
       	resolve(a);
    });
    return p;            
}
fn1(11)
.then(function(a){
		if(a > 10){
	   		console.log('a大于10')
   		}
   		return b
})
.catch(function (err){
	console.log('发生了错误:' + err)
})
```

这段代码中 本来想return a的 结果写成了b，正常来说浏览器会报错，不会向下执行了，在我们用了catch后，

浏览器会打印出**a大于10**和**发生了错误:ReferenceError: b is not defined**。

也就是说即使是上面出错了，还是进到catch方法里面去了，而且把错误原因传到了err参数中，使得程序继续执行下去。

### all

all方法提供了多个任务并行，执行异步操作的能力，并且在所有异步操作执行完后才执行回调。

- all方法接收的参数是一个数组，其中每个对象都是promise

```js
let p = Promise.all([fn1, fn2, fn3])

p.then(funciton(){
  // 三个都成功则成功  
}, function(){
  // 只要有一个失败，则失败 
})
```

有了all，我们就可以并行执行多个异步操作，有一个场景是很适合用这个的，打开一个网页，需要加载各类资源，所有的都加载完后，我们再进行页面的初始化。

### race

all方法的效果实际上是**谁跑的慢，以谁为准执行回调**，那么相对的就有另一个方法**谁跑的快，以谁为准执行回调**，这就是race方法，这个词本来就是赛跑的意思。

拿上面的fn123举例子，假如他们分别是1、2、3秒执行完，那么在第一秒结束的时候就会输出fn1执行后的结果，在两秒跟三秒的时候会分别输出fn2、fn3的结果。

这个race有什么用呢？使用场景还是很多的，比如我们可以用race给某个异步请求设置超时时间，并且在超时后执行相应的操作。

例如图片请求，我们将一个延迟请求（假如是3秒）跟图片请求同时使用race方法调用，在3秒的时候如果请求成功了，就会resolve进入then方法，如果失败了就会进入catch方法输出图片资源请求失败的错误。

## 最后

十一的假期结束了，上学人又要继续上课了，害。

> 点个赞，一起努力进步吧♥
