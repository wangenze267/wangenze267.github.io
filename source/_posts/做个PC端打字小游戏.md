---
title: 做个PC端打字小游戏
author: Ned
tag:
  - 整活系列
  - JavaScript
abbrlink: 2621331394
---
## 前言
代码不光是我们用来工作的，也应该是我们用来娱乐的，今天就带着小伙伴们整个活！

看完这篇文章，你会学会如何整活~

小时候我记得有个软件叫做`金山打字通`，里面有个打字的飞机大战不知道有没有小伙伴玩过，当然我整不来他那么优秀，我只能做一个较为简单的（**低配版**），低的好像还真挺低。

先来看看效果吧：

![打字游戏效果图.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/408057b8b3e548bf9f6715d512324fe6~tplv-k3u1fbpfcp-watermark.image?)

## 开始整活

页面构成比较简单，一个是我们要打的字母，一个是下面的那行小字，用来做提示用。
```html
     <div id="char">A</div>
     <div id="result">请在按键上按下屏幕上显示的字母</div>
```
接下来是做一些简单的布局，就是将内容居中，颜色等做一下调整，我们先贴代码，完后在进行一个细说。
```css
    body{
        margin: 0;
        /*开启弹性布局，并让弹性布局中的子元素
        水平居中对齐，垂直居中对齐*/
        display: flex;
        /* 用于设置或检索弹性盒子元素在主轴（横轴）方向上的对齐方式 */
        justify-content: center;
        align-items: center;
        /*文字居中*/
        text-align: center;
        /*设置背景颜色的经向渐变*/
        background: radial-gradient(circle,#444,#000,#000);
    }
    #char{
        font-size: 400px;
        /*color: lightgreen;*/
        color: #90EE90;
        /*设置文字阴影*/
        /*text-shadow: 水平位置 垂直位置 模糊距离 阴影颜色*/
        /*位置可以为负值*/
        text-shadow: 0 0 50px #666;
    }
    #result{
        font-size: 20px;
        color: #888;
    }
    /*找到id为char及类名为error的div元素*/
    #char.error{
        color: red;

    }
```

我们定义了一个`error`类，用来做用户输入错的时候，将字母变成红色来给与用户提示。

背景用的径向渐变也挺有意思的，你必须要设定两个终止色，由中心到四周产生渐变色的效果，他的第一个参数有两种情况，椭圆跟圆，我们是可以自己进行选择的。

如果对此感兴趣也可以去菜鸟教程[径向渐变](https://www.runoob.com/cssref/func-radial-gradient.html)看一看。

接下来我们来写我们的js逻辑处理。

先定义和获取我们需要的变量跟Dom节点
```js
    //表示正确的次数
    var okCount=0;
    //错误的次数
    var errorCount=0;
    //获取显示字符的div
    var charBox=document.getElementById('char');
    //获取显示结果的div
    var result=document.getElementById('result');
```
正确率=正确的次数/总次数，我们再写一个函数来显示判断结果，显示正确跟错误的个数，还有正确率。
```js
    //展示计算的结果
    function showResult(){
        var rate=100*okCount/(okCount+errorCount);
        //显示正确个数 错误个数 及正确率
        result.innerHTML='正确'+okCount+'个'+'错误'+errorCount+'个'+'正确率'+rate.toFixed(2)+'%';
    }
```
>toFixed(2)的作用是保留两位小数

我们再写一个函数去随机获取A~Z这些英文字母的值。
```js
var code = 0; // 存放随机数
 //获取A~Z之间的任意一个字符
    function show(){
        //获取[0,1)之间的一个随机数
        var rand=Math.random();
        //获取一个0到26之间的一个随机数（不包含26）
        code=rand*26;
        //Math.floor(a)对数字a向下取整，获取到一个小于等于a最近的整数
        //获取0~25之间任意一个整数
        code=Math.floor(code);
        //获取到65~90之间的任意整数
        code=code+65;
        //把ASCII的十进制编码转化成对应的字符
        //获取A~Z的任意一个字符
        var char=String.fromCharCode(code);
        //把字符显示在界面上
        charBox.innerHTML=char;
    }
```
利用随机数生成A~Z的ASCII码值，完后用`String.fromCharCode`转成英文字母并显示到页面上。

还有最后一个步骤就是监听用户的键盘按键事件,利用`window.onkeydown`事件来做。
```js
    //键盘按下来的事件
    window.onkeydown=function(ev){
        //获取按键所对应的ASCII编码
        var key=ev.keyCode;
        //判断按键字母所对应的数字和随机获取的数字是否相等
        if(key==code){
            //按键正确，正确次数+1
            okCount ++;
            //当按键正确时，重新显示新的字符
            show();
        }else{
            //按键错误，错误次数+1
            errorCount ++;
        }   
        showResult();
    }
```

现在已经成功了，来看看效果吧！

![打字游戏效果.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a242ddcc4630449f99bfa08c864b99bb~tplv-k3u1fbpfcp-watermark.image?)

但是我们优秀的程序员当然想给用户**略微**（更好）的用户体验！所以我们来引入一个[Animate.css](https://animate.style/)动画库。

结合我们的小游戏，选择了`zoomIn`与`shake`两个动画，一个作为英文字母的出现伴随动画，另一个作为错误的时候提示用户的动画。我们在监听用户按键做出判断的时候来给我们的元素加上相对应动画的类名，并且在0.5s过后移除对应的类名就行了，防止发生冲突。
```js
    //键盘按下来的事件
    window.onkeydown=function(ev){
        //获取按键所对应的ASCII十进制编码
        var key=ev.keyCode;
        //判断按键字母所对应的数字和随机获取的数字是否相等
        if(key==code){
            //按键正确，正确次数+1
            okCount ++;
            //当按键正确时，重新显示新的字符
            show();
            //添加正确的动画 通过js给div添加类名
            charBox.className ='animated zoomIn';
        }else{
            //按键错误，错误次数+1
            errorCount ++;
            //添加按键错误的动画
            charBox.className='animated shake error';
        }
        showResult();
        //0.5秒之后清除动画
        setTimeout(clearAnimated,500);
    }
    function clearAnimated(){
        //负责清除动画
        charBox.className='';
    }
```
到此为止，我们今日的整活就结束啦~

## 最后
希望大家快乐起来呀，老想着如何去实现产品经理给的需求会十分疲惫，在空闲之余不如来用我们擅长的代码整个活~

让自己，也让周围的人开心开心，也能让自己的心情变得轻松起来！

本文首发于[掘金 ](https://juejin.cn/user/105972016875911)。