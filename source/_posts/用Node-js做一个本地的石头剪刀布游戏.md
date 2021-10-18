---
title: 用Node.js做一个本地的石头剪刀布游戏
tags:
  - Node.js
  - 经历
abbrlink: 2316163776
date: 2021-10-02 23:59:41
---

### 前言

前一段日子学了个石头剪刀布游戏，自己在本地进行了实现，想挂在自己服务器上让他形成一个外网可访问的游戏的时候，出了问题，是接口请求路径不对的问题，现在还不知道什么原因，等解决之后我还会更一下。

<!--more-->

### 所需要准备的

- Node.js环境（没有的可以去官网下一下，傻瓜式安装就好）
- 基础的html、css、js能力
- 入门级的Node.js就好（因为我也是这个级别）
- 一个你熟悉的代码编写工具

### 开始上手操作

首先我们需要一个html页面来作游戏结果的返回以及玩家操作。

**需求分析：**

- 我们需要一个地方来做游戏结果的返回
- 还需要三个按钮来给用户做操作交互

下面来看`index.html`文件

```html
<div id="output" style="height: 400px; width: 600px; background: #eee"></div>
<button id="rock" style="height: 40px; width: 80px">石头</button>
<button id="scissor" style="height: 40px; width: 80px">剪刀</button>
<button id="paper" style="height: 40px; width: 80px">布</button>
```

我定义了一个`div`，来作为显示游戏结果的地方，定义了三个按钮，分别代表剪刀、石头、布。

接下来我们应该做的就是通过接口的方式，提交我们用户的操作并且获取游戏结果，将他显示在刚刚的`div`里。

```js
const $button = {
        rock: document.getElementById('rock'),
        scissor: document.getElementById('scissor'),
        paper: document.getElementById('paper')
    }

const $output = document.getElementById('output')

Object.keys($button).forEach(key => {
    $button[key].addEventListener('click', function () {
        fetch(`http://${location.host}/game?action=${key}`)
            .then((res) => {
                return res.text()
            })
            .then((text) => {
                $output.innerHTML += text + '<br/>';
            })
    })
})
```

之后我们去建立一个`game.js`文件，写一下游戏的判断逻辑。

```js
module.exports = function (palyerAction){
    if(['rock','scissor','paper'].indexOf(palyerAction) == -1){
        throw new Error('invalid playerAction');
    }
    // 计算出电脑出的结果
    var computerAction;
    var random = Math.random() * 3;
    if(random < 1){
        computerAction = "rock"
    }else if(random > 2){
        computerAction = "scissor"
    }else{
        computerAction = "paper"
    }
    if(computerAction == palyerAction){
        return 0;
    }else if(
        (computerAction == "rock" && palyerAction == "scissor") ||
        (computerAction == "scissor" && palyerAction == "paper") ||
        (computerAction == "paper" && palyerAction == "rock")
    ){
        return -1;
    }else{
        return 1;
    }
}
```

大致的逻辑很简单，通过随机数让电脑出拳，之后判断胜负并返回。

下面看一下用`node.js`写的简单交互的地方：

```js
const querystring = require('querystring');
const http = require('http');
const url = require('url');
const fs = require('fs');

const game = require('./game');

let playerWon = 0;

let playerLastAction = null;
let sameCount = 0;

http
    .createServer(function (request, response) {
        const parsedUrl = url.parse(request.url);
        if (parsedUrl.pathname == '/favicon.ico') {
            response.writeHead(200);
            response.end();
            return;
        }
        if (parsedUrl.pathname == '/game') {
            const query = querystring.parse(parsedUrl.query);
            const playerAction = query.action;
            if (playerWon >= 3 || sameCount == 9) {
                response.writeHead(500);
                response.end('我再也不和你玩了！');
                return
            }
            if (playerLastAction && playerAction == playerLastAction) {
                sameCount++;
            } else {
                sameCount = 0;
            }
            playerLastAction = playerAction
            if (sameCount >= 3) {
                response.writeHead(400);
                response.end('你作弊！');
                sameCount = 9;
                return 
            }
            // 执行游戏逻辑
            const gameResult = game(playerAction);
            // 先返回头部
            response.writeHead(200);
            // 根据不同的游戏结果返回不同的说明
            if (gameResult == 0) {
                response.end('平局！');
            } else if (gameResult == 1) {
                response.end('你赢了！');
                // 玩家胜利次数统计+1
                playerWon++;
            } else {
                response.end('你输了！');
            }
        }
        // 如果访问的是根路径，则把游戏页面读出来返回出去
        if (parsedUrl.pathname == '/') {
            fs.createReadStream(__dirname + '/index.html').pipe(response);
        }
    })
    .listen(3000)
```

在cmd窗口中输入`node index.js`就可以在浏览器的`localhost:3000`端口中看见这个游戏啦！

> 有一些node基础的同学们应该看起来很容易，毕竟我也不咋会emm。

那，来看一下效果吧。

> 没有做丝毫美化，实在是懒欸。

![](石头剪刀布1.gif)



### 最后

日后想优化一下，挂到自己服务器上，嘿嘿，好歹是自己做的第一个小游戏~

> 大家有什么好的建议嘛~

