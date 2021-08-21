---
title: 如何写好JavaScript（下）
author: Ned
tag:
  - 字节青训营
  - JavaScript
abbrlink: 3177970664
---

### 前言

在打开这个视频之前，我真没想到讲的是优化算法之类的东西，可能是[如何写好JavaScript（上 ）](https://blog.wangez.site/posts/98016397.html/#more)迷惑了我。

有听懂的，有没懂的，先记下来，留着梳理。

### 看一段代码

```javascript
// 判断一个mat2d矩阵是否是单位矩阵
function isUnitMatrix2d(m){
    return m[0] === 1 && m[1] === 0 && m[2] === 0 && m[3] === 1 && m[4] === 0 && m[5] === 0;
}
```

这段代码好不好？ 为什么？

<!--more-->

> 这个代码从优雅程度上来说确实是有些过于长了，看起来不是十分的美观，我们可以使用类似于for循环的方式将其变得美观起来。
>
> 但是如果他是写在代码底层（spritejs）的话，这么写的性能是最好的，因为他直接拿到索引就可以来比较，是循环所比拟不了的。

**月影大佬：我们所有的代码风格，都是根据场合来的，如果是一些框架或者开源的库中，看见了不是很优雅的代码，我们应该结合场景去分析，因为有的时候这么写是有理由的。**

写代码最应关注什么？

- 风格：

  > 很重要，尤其是多人协作。关键在于风格要统一，否则会增加代码维护的成本。

- 效率：

  > 要在心中明白，什么样子的代码效率是最高的，尽量结合场景选择最合适的代码（效率or可读性）。

- 约定

- 使用场景

- 设计

### **当年的left-pad事件**

```javascript
function leftpad(str, len, ch){
    str = String(str);
    var i = -1;
    if(!ch && ch !== 0) ch=" ";
    len = len - str.length;
    while(++i<len){
        str = ch + str;
    }
    return str;
}
```

#### **事件本身的槽点**

- NPM模块粒度

  > 说npm模块粒度太细了，单个函数的模块。
  >
  > 当年模块化跟打包工具不是很成熟

- 代码风格

  > 可读性还好

- 代码质量/效率

  > 这个效率还可以，因为正常场景中不会拼接太长的字符串

#### **提高效率与代码简洁性**

```js
function leftpad(str, len, ch){
    str = "" + str;
    const padLen = len - str.length;
    if(padLen <= 0){
        return str;
    } else{
        return(""+ch).repeat(padLen) + str;
    }
}
```

### **交通灯：状态切换**

实现一个切换**多个**交通灯**状态切换**的功能

#### 版本一

> 不知道为什么听这个课，我现在觉得版本一就等于很垃圾

```js
const traffic = document.getElementById('traffic');
(function reset(){
    traffic.className = 's1';
    setTimeout(function(){
        traffic.className = 's2';
        setTimeout(function(){
            traffic.className = 's3';
            setTimeout(function(){
                traffic.className = 's4';
                setTimeout(function(){
                    traffic.className = 's5';
                    setTimeout(reset,1000)
                },1000)
            },1000)
        },1000)
    },1000);
})();
```

> setTimeout本身是异步，将代码这样嵌套，会造成回调地狱的问题，而且代码很丑，可读性差，不方便维护。
>
> 看来跟我上边说的一样，版本一，都是垃圾，哈哈哈哈

#### 版本二

```js
const traffic = document.getElementById('traffic');
const stateList = [
    {state: 'wait', last: 1000},
    {state: 'stop', last: 3000},
    {state: 'pass', last: 3000},
];

function start(traffic,stateList){
    function applyState(steteIdx){
        const {state,last} = stateList[stateIdx];
        traffic.className = state;
        setTimeout(()=>{
            applyState((stateIdx + 1) % stateList.length);
        },last)
    }
    applyState(0);
}
start(traffic,stateList);
```

> 采用的函数封装的方法

#### 版本三

```js
const traffic = document.getElementById('traffic');
function poll(...fnList){
    let stateIndex = 0;
    return function(...args){
        let fn = fnList[stateIndex++ % fnList.length];
        return fn.apply(this, args);
    }
}
function setState(state){
    traffic.className = state;
}
let trafficStatePoll = poll(setState.bind(null,'wait'),
setState.bind(null,'stop'),setState.bind(null,'pass'));
setInterval(trafficStatePoll,2000);
```

> 抽象出一个轮巡的方法，采用高阶函数

#### 版本四

```js
const traffic = document.getElementById('traffic');
function wait(time){
    return new Promise(resolve => setTimeout(resolve,time));
}
function setState(state){
    traffic.className = state;
}
async function start(){
    while(1){
        setState('wait');
        await wait(1000);
        setState('stop');
        await wait(3000);
        setState('pass');
        await wait(3000);
    }
}
start();
```

> 知道了异步，就肯定好做了呀，用async和await，命令式的写法。
>
> 可以继续保持异步采用声明式的封装继续去做出版本567.

### **判断是否是4的幂**

#### 版本一

```js
function isPowerOfFour(num){
    num = parseInt(num);
    while(num>1){
        if(num%4)return false;
        num /= 4;
    }
    return true;
}
```

> 很中规中矩的写法，每次对4取余，结果还是要能被4整除。

#### 版本二

```js
function isPowerOfFour(num){
    num = parseInt(num);
    while(num>1){
       if(num& 0b11) return false;
       num >>>=2;
    }
    return true;
}
```

> 将对4取余数变成了判断二进制数的后两位，后两位是00就肯定是4的倍数，除以四变成右移两位。

#### 版本三

```js
function isPowerOfFour(num){
    num = parseInt(num);
    return num > 0 &&
        (num&(num - 1)) === 0 &&
        (num& 0xAAAAAAAA) === 0;
}
```

> 刚刚是 log的复杂度算法，这个是常数复杂度的算法
>
> 如果一个数是4的幂的话，转化二进制肯定只有首位是1的，并且后面跟着偶数个0，所以在这种情况下，判断只有一个1，并且还有偶数个0即可。

#### 版本四

```js
function isPowerOfFour(num){
    num = parseInt(num).toString(2);
    return /^1(?:00)*$/.test(num);
}
```

> 理论与上一个同理，但是我真的震惊了，原来一个思路可以有这么多种解决方法。
>
> 但是相对来说，正则的开销会比数值的要大，但是在这个情景中还是可以接受的。

### **洗牌**

简单实现

```js
const cards = [0,1,2,3,4,5,6,7,8,9];
function shuffle(cards){
    return [...cards].sort(()=> Math.random() > 0.5 ? -1 : 1);
}
console.log(shuffle(cards));
```

> 如果这是个抽奖程序的话，是会被打的。
>
> 因为不公平。和原来的位置相关。

我们来验证一下

```js
const cards = [0,1,2,3,4,5,6,7,8,9];
function shuffle(cards){
    return [...cards].sort(()=> Math.random() > 0.5 ? -1 : 1);
}

const result = Array(10).fill(0);

for(let i = 0; i < 10000; i++){
    const c = shuffle(cards);
    for(let j = 0;j < 10; j++){
        result[j] += c[j];
    }
}
console.log(result);
```

我们来调用上面的洗牌算法一万次，完后将每个位置上出现牌的点数都相加，如果他公平，我们相加的结果应该都差不多，对吧。

>[
>  38511, 38548, 45148,
>  46416, 46551, 44174,
>  43456, 46877, 48894,
>  51425
>]

这确是我打印出来的数据，连续跑了好几组都是某某要大，你将一万次改的越大，最后位置的数字就会越大，也就是说，小数字会出现在靠前的位置，大数字会出现在靠后的位置。

如果还不确认的小伙伴可以去做一下概率统计。

> 这是因为sort会两两交换，每个位置上交换的次数是不同的，所以最后的数字在交换到前面的概率是比较小的，当次数被无限放大的时候，这个差距就是越来越大。所以导致刚刚的洗牌算法是不公平的。

如果想要公平的话，有一种算法是这样的，每次随机抽取一张牌把它放到最后的位置去。

```js
const cards = [0,1,2,3,4,5,6,7,8,9];
function shuffle(cards){
    const c =[...cards];
    for(let i = c.length; i>0; i--){
        const pIdx = Math.floor(Math.random() * i);
        [c[pIdx],c[i-1]] = [c[i-1], c[pIdx]];
    }
    return c;
}

const result = Array(10).fill(0);

for(let i = 0; i < 10000; i++){
    const c = shuffle(cards);
    for(let j = 0;j < 10; j++){
        result[j] += c[j];
    }
}
console.log(result);
```



>[
>  45466, 44956, 44753,
>  45253, 45179, 44765,
>  45147, 44469, 45404,
>  44608
>]

可以看出这个打印出来的结果，大小都是差不大多的。

#### **分红包**

```js
function generate(amount, count){
    let ret = [amount];
    while(count > 1){
        // 挑选出最大的一块进行切分
        let cake = Math.max(...ret),
            idx = ret.indexOf(cake),
            part = 1 + Math.floor((cake/2) * Math.random()),
            rest = cake - part;
        ret.splice(idx,1,part,rest);
        count--;
    }
    return ret;
}
```

不停的切分最大的金额。

> 这个会带来一个问题，就是金额很平均。
>
> 不会有人拿了很大的红包，少了一丝趣味性。
>
> 不够刺激~

### 最后

听君一席话，如听一席话。他好似讲了许多，又好像什么也没讲。

是我菜了，继续加油！
