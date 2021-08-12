---
title: Node.js学习笔记之异步编程部分（一）
author: Ned
tags: Node.js
abbrlink: 3168823204
---

# Node.js学习笔记之异步编程部分（一）

- 回调函数格式规范
  - error-first callback
  - Node-style callback

- 第一个参数是error，后面的参数才是结果

### 场景

你在面试之后，面试官让你回去等消息。

<!-- more -->

结果：

- 过
- 不过

ps：面试过了我们就笑一下，面试不过我们就哭！

先上代码：

```javascript
	interview(function(err){
		if(err){
			return console.log('cry at 1st');
		}
		interview(function(err){
			if(err){
				return console.log('cry at 2nd');
			}
			interview(function(err){
				if(err){
					return console.log('cry at 3rd');
				}
				console.log('smile');
			});
		})
	})
function interview(callback) {
	setTimeout(()=>{
		if(Math.random() < 0.8){
			callback('success');
		} else {
			callback( new Error('fail')); 
		}
	},1000)
}
```

这是一个正常的面试流程：

- 一面------>挂掉
- 一面------->二面------>挂掉
- 一面------->二面------>三面------>挂掉
- 一面------->二面------>三面------>入职

看是看着这种嵌套是真的恶心，这种问题就叫做`回调地狱`，这也是比较容易出现在异步中的问题，另外一个问题就是并发了，我们用同时面试两家公司为例，毕竟这也是正常能够出现的现象。

这时候张三说了：我会写我会写，还是上面代码的理念，我加一个计数器，面试过了就count++，count为2的时候，就证明两个面试都过了，这样可以用if判断并写一些逻辑。

简单描述一下张三的代码结构：

```javascript
var count = 0;
interview(function (err) {
    if(err){
        return console.log('cry');
    }
    count++
})
interview(function (err) {
    if(err){
        return console.log('cry');
    }
    count++
    if(count){
        
    }
})
```

大致是这个样子，看起来很麻烦。这个问题就涉及到异步流程控制问题，也就是异步的并发，针对这个问题，我们强大的社区当然是有解决方案的。

- npm包：`async.js`

- thunk编程方式

> 现在好像这两个方式都不用了😭

