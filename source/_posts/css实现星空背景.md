---
title: CSS实现星空背景
author: Ned
tag:
  - CSS
  - 整活系列
abbrlink: 2834145920
---
## 前言

最近真是越来越对CSS感兴趣了，于是再来整一手，夜晚的星星，再配合上皎洁的月光，这唯美的星空，它来了！

今天带领大家，用CSS实现一下，这美丽的星空。

## 开始实现星空

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/925111aead044598964e7536556d8e57~tplv-k3u1fbpfcp-watermark.image?)
我是找了张图片，这毕竟功力有限，目前还不能人造星~，下面说一下如何将它放置在夜空中，并实现眨眼睛的效果：

运用了一个`span`标签，将此图片作为其背景图，来生成星星：

```js
var screenW = document.documentElement.clientWidth;
var screenH = document.documentElement.clientHeight;
for(var i=0; i<150; i++){
    var span = document.createElement('span');
    document.body.appendChild(span);
    var x = parseInt(Math.random() * screenW);
    var y = parseInt(Math.random() * screenH);
    span.style.left = x + 'px';
    span.style.top = y + 'px';
    span.style.zIndex = "0";
    var scale = Math.random() * 1.5;
    span.style.transform = 'scale('+ scale + ', ' + scale + ')';
}
```

先获取屏幕宽高，完后用上随机数使得星星的位置随机，大小随机，频率随机。

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d91ceb0ed1724c868e47428d6f90fa9e~tplv-k3u1fbpfcp-watermark.image?)
## 会眨眼睛的才是好星星
星星还要是一眨一眨的，才好看，所以我们给它加上一个动画，更改其的透明度就好：

```css
@keyframes flash {
    0%{opacity: 0;}
    100%{opacity: 1;}
}
```

但是我们很快发现一个问题，就是它太过于整齐划一：
![整齐划一.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/542cb54cebde4496a99b7147ec3ddebf~tplv-k3u1fbpfcp-watermark.image?)

我们在生成星星的时候，给它每一个的延迟频率随机一下，这样就能保证它们有一种参差错落的感觉。
```js
var rate = Math.random() * 1.5;
span.style.animationDelay = rate + 's';
```
现在来看看我们美丽的星空吧~：

![星空.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2ac35929aa1146b1afd954acbaa6fbc1~tplv-k3u1fbpfcp-watermark.image?)

最后再给星星加一个`hover`效果，将其放大一点，完后旋转个180

```css
span:hover{
    transform: scale(3, 3) rotate(180deg) !important;
    transition: all 1s;
}
```

![hover.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/28caf897e26f482cb17289c70f6295a1~tplv-k3u1fbpfcp-watermark.image?)
## 开始实现月亮
一个美丽的夜晚，天空中除了星星，应当还有月亮：月亮主要是两个动画，一个是从左下往右上移动，达到一个`月亮升起`的效果，另一个是随着升起，月亮周围的光辉变得越来越亮眼。

做法：将月亮放到一个容器中，用容器来做移动的特效，月亮本身只关注光辉就好。
```html
<div id="wrapper">
        <div id="circle"></div>
</div>
```
```css
#wrapper {
    position:absolute;
    top:50px;
    left:80%;
    width:200px;
    height:200px;
    margin-left:-100px;
    animation: moonline 5s linear;
}
@-webkit-keyframes moonline {
    0% {top:250px;left:0%;opacity:0;}
    30% {top:150px;left:40%;opacity:0.5;}
    80% {top:50px;left:80%;opacity:1;}
}
#circle {
    position: absolute;
    top: 0;
    left: 0;
    width: 200px;
    height: 200px;
    background-color: #EFEFEF;
    box-shadow:0 0 40px #FFFFFF;
    border-radius: 100px;
    animation:moonlight 5s linear;

}
@-webkit-keyframes moonlight {
    0% {-webkit-box-shadow:0 0 10px #FFFFFF;}
    30% {-webkit-box-shadow:0 0 15px #FFFFFF;}
    40% {-webkit-box-shadow:0 0 20px #FFFFFF;}
    50% {-webkit-box-shadow:0 0 25px #FFFFFF;}
    100% {-webkit-box-shadow:0 0 30px #FFFFFF;}
}
```
看一下最终效果：

![月亮升起.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/27e8107acff34caab4778e8b464e78a5~tplv-k3u1fbpfcp-watermark.image?)