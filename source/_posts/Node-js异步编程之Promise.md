---
title: Node.js异步编程之Promise
author: Ned
tags: Node.js
abbrlink: 697311807
date: 2021-08-15 00:22:58
---

### 前言

先来介绍一下Promise是什么？

Promise：

- 当前事件循环得不到的结果，但未来的事件循环会给到你结果
- 是一个状态机
  - pending------还没有得到结果
  - fulfilled/resolved------得到了一个正确的结果
  - rejected------得到了一个错误的结果
  
  <!-- more -->

![](状态.png)

### 从代码入手👻

```javascript
(function(){
    var promise = new Promise(function(resolve,reject){
        setTimeout(()=>{
            resolve();// reject(new Error());
        },500)
    })
    
    console.log(promise);
    
    setTimeout(()=>{
        console.log(promise);
    },800)
})();
```

这是一个从pending到resolved/rejected的过程。

那么可能有的小伙伴会想，我可不可以让他在300毫秒的时候转变成resolve的状态，在500毫秒的时候从resolved转变成rejected状态呢？那我们来实际操作一下看看结果吧。

> 我也有这样的疑惑

```javascript
(function(){
    var promise = new Promise(function(resolve,reject){
        setTimeout(()=>{
            resolve();
        },300)
        setTimeout(()=>{
            reject(new Error());
        },300)
    })
    
    console.log(promise);
    
    setTimeout(()=>{
        console.log(promise);
    },800)
})();
```

![](浏览器.png)

从结果我们可以看到，他还是fulfilled状态，也就是resolved状态，没有改变。

**所以在resolved状态与rejected状态之间应该是不能转换的。**

### 继续学习🐸

我们使用promise去解决异步问题，是希望他拿到结果之后就要立即通知我们，那这个时候我们要怎么去写呢？

我们可以借助promise下的一个方法，叫做**then**

把上面的代码进行改写

```javascript
(function(){
    var promise = new Promise(function(resolve,reject){
        setTimeout(()=>{
            resolve(3);
        },300)
    }).then(function(res){
    	console.log(res)
    }).catch(function(){
    
    })
    
    console.log(promise);
    
    setTimeout(()=>{
        console.log(promise);
    },800)
})();
```

可能有小伙伴注意到了，我们上面还多了一个catch方法，没错，他在这里也是用来处理error的，而error在promise里就对应着rejected状态了，将上面的代码再改点东西，就成了这样子。

```javascript
(function(){
    var promise = new Promise(function(resolve,reject){
        setTimeout(()=>{
            reject(new Error());
        },300)
    }).then(function(res){
    	console.log(res)
    }).catch(function(err){
        console.log(err)
    })
    
    console.log(promise);
    
    setTimeout(()=>{
        console.log(promise);
    },800)
})();
```

- 关于.then和.catch
  - resolved状态的promise会回调后面的第一个.then
  - rejected状态的promise会回调后面的第一个.catch
  - 任何一个rejected状态且后面没有.catch的promise，都会造成浏览器/node环境的全局错误

### 为什么说promise优秀呢？

他可以解决异步流程控制的问题（这个在前面callback那节笔记有记过，[可以去看一下]([Node.js学习笔记之异步编程部分（一） | Wangez-Blog](https://blog.wangez.site/posts/3168823204.html/))）。

还是之前面试的问题，这次我们用promise的思路来看一下

```javascript
(function(){
    var promise = interview();
    promise
    .then((res)=>{
        console.log('smile');
    })
    .catch((err)=>{
        console.log('cry')
    })

})();


function interview(){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            if(Math.random()>0.2){
                resolve('success')
            }else{
                reject(new Error('fail'));
            }
        },500)
    })
}
```

### 小细节，拿笔记好📢

执行then和catch会返回一个新的promise，该promise最终状态根据then和catch的回调函数的执行结果决定

- 如果回调函数是throw，那么promise就是rejected状态
- 如果回调函数是return，那么promise就是resolved状态
- 但如果回调函数最终return了一个promise，该promise回跟回调函数return的那个promise状态保持一致。

接下来用多轮面试的例子，来演示一下：

```javascript
(function(){
    var promise = interview();
    promise
    .then((res)=>{
        return interview(2)
    })
    .then(()=>{
        return interview(3)
    })
    .then(()=>{
        console.log('smile');
    })
    .catch((err)=>{
        console.log('cry at  ' + err.round + '  round');
    })

})();


function interview(round){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            if(Math.random()>0.2){
                resolve('success')
            }else{
                var error = new Error('fail');
                error.round = round
                reject(error);
            }
        },500)
    })
}
```

想知道你在第几轮面试挂了吗？去执行一下试试吧！

### 小优化🎁

我们肯定是同时面试多家公司的呀，那我们再给上面的代码做一些优化吧。

```javascript
(function(){
    Promise
    .all([
        interview('tencent'),
        interview('Ali88')
    ])
    .then(()=>{
        console.log('smile');
    })
    .catch((err)=>{
        console.log('cry for  ' + err.name );
    })

})();


function interview(name){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            if(Math.random()>0.2){
                resolve('success')
            }else{
                var error = new Error('fail');
                error.name = name
                reject(error);
            }
        },500)
    })
}
```

注意：

- catch只能返回第一个面试挂的公司，如果都想知道就要存好所有的promise的状态并且打印出来哦。

### 最后

肝不动了，睡了睡了，总觉得上面那个例子，面试通过一家，第二家挂了他会给我返回到catch里面去呢，明天在研究一下。

