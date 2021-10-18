---
title: Node.js之事件循环
author: Ned
tags: Node.js
abbrlink: 3309484712
---

### 简单叙述一下什么是事件循环

还是拿之前的餐厅来举例，我们去点了个番茄炒蛋，服务生告诉后厨做一个番茄炒蛋，张三也去了餐厅，点了个麻辣小龙虾，服务生又告诉后厨做一份麻辣小龙虾，小李也去了，点了个蛋炒饭。

<!-- more -->

解析一下这个情景，餐厅是一个事件循环，其中我们的点餐叫做线程。服务生第一个把我要的番茄炒蛋端过来了，于是我的线程结束了，因为麻辣小龙虾做的慢，蛋炒饭做完了，服务生又给小李端上来了蛋炒饭，小李虽然是后来的，但是他的过程要比张三快，最后小龙虾好了，服务生给张三端上来小龙虾，至此三个线程都结束了。

### 用代码实现一个简易版的事件循环

```javascript
const eventloop = {
    // 事件队列
    queue: [],
    loop(){
        while(this.queue.length){
            // 如果有事件就进行处理
            var callback = this.queue.shift();
            callback();
        }
        // 每50毫秒会检测一下事件
        setTimeout(this.loop.bind(this),50);
    },
    // 添加事件
    add(callback){
        this.queue.push(callback);
    }
}

eventloop.loop();

// 分别在 500毫秒跟800毫秒，往队列中加入两个事件
setTimeout(()=>{
    eventloop.add(function(){
        console.log('第一个');
    })
},500)
setTimeout(()=>{
    eventloop.add(function(){
        console.log('第二个');
    })
},800)
```

### 有话说

当然，在真实情况中会比这复杂的多，不会每50毫秒去检测一次，要快的多，添加的事件也不会只有回调函数，例如一些文件读写操作等。我们的事件队列可能还会有文件处理队列，定时操作队列等多种不同的队列。

就先到这，下次见~

