---
title: 复习CSS盒模型
author: Ned
tag:
  - CSS
  - 面试专栏
abbrlink: 2223466213
---
>最近在准备春招了，复习一些基础的知识点，整合成一个专栏发出来，希望可以帮助到更多的小伙伴们~🥰
# 盒模型📦

先用一张图来说明一下我会怎么来介绍盒模型：
![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b8ea3868d0cf43a297314e611458d241~tplv-k3u1fbpfcp-watermark.image?)

## 什么是盒模型🍇
其实我们大家都能经常看见它，尤其是我们前端的小伙伴们，在浏览器中打开`f12`就能看见这样一个动态变化的图。

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7e953a78cb6249f0ae909ae3fa76f88e~tplv-k3u1fbpfcp-watermark.image?)
它其实就是我们这篇文章的主角-盒模型。由这张图可以看出，盒模型包含了`margin`、`border`、`padding`、`content`这四个部分。

>所有HTML元素都可以看作盒子，而我们平时就是盒子的搬运工。
## 介绍标准模型和IE模型，以及他们的区别🍈
它俩的**区别就一个，计算宽度（高度）的方式不一样。**

- 标准盒模型

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7250e5711f3641c9a1070b7769016e2a~tplv-k3u1fbpfcp-watermark.image?)
width = content

总宽度 = width + margin + border padding 
> margin包含mrgin-left与margin-right 其他同理

- IE盒模型

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8bbfd356eec244cc9db08952803f0c4f~tplv-k3u1fbpfcp-watermark.image?)
width = content + border + padding

总宽度 = width + margin
## CSS如何设置盒模型，以及计算对应的宽和高🍉
css中有一个属性：`box-sizing`，我们可以通过这个属性去设置标准盒模型(`content-box`)或者是IE盒模型(`border-box`)，默认为标准盒模型。
详细参数可以去W3c去看一下，此处不做说明：[ box-sizing ](https://www.w3school.com.cn/cssref/pr_box-sizing.asp)

**如何去计算元素的宽（高）？** 我们在得知它是哪种盒模型之后就可以依据我们上文的公式去计算了，可以打开`F12`，滑到图那里，去查阅该元素四部分（`margin`、`border`、`padding`、`content`）的值是多少，完后进行计算即可。
## js如何设置获取盒模型对应的宽和高🍍
- dom.style.width/height
这个方法只能获取写在行内样式中的宽度，写在style标签中和使用`link`外链都是获取不到的，我们下面来看一下：
```html
<div id="div">这是一个div</div>
```
```css
div{
    width:300px;
    margin-left:50px;
    border:25px;
    padding-right:60px;
    background-color:pink;
}
```
```js
let divWidth = document.getElementById("div").style.width;
console.log(divWidth);
```
我将样式写在了style标签内，看看他是否能打印出来`div`的宽度。

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7d85fdc694b245eca49c4c50ba08f699~tplv-k3u1fbpfcp-watermark.image?)
是打不出来的，那在行内写一个宽度为`100px`再试试。

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/83deed873b5d4f8f8eb3fbe3a3a52bdf~tplv-k3u1fbpfcp-watermark.image?)
成功将宽度打印了出来。

如此之外还有三个api可以使用：
- `dom.currentStyle.width/height` 取到的是最终渲染后的宽和高，只有IE支持此属性。
- `window.getComputedStyle(dom).width/height` 同上一个但是多浏览器支持，IE9以上支持
- `dom.getBoundingClientRect().width/height` 也是得到渲染后的宽和高，大多浏览器支持。IE9以上支持，除此外还可以取到相对于视窗的上下左右的距离。
## 根据盒模型解释边距重叠🍓
当两个外边距相遇时，他们将形成一个外边距，合并后的外边距高度等于两个发生合并的外边距的高度中的较大者。

**注意**：只有普通文档流中块框的垂直外边距才会发生外边距合并，行内框、浮动框或绝对定位之间的外边距不会合并。

再用一个demo来说明一下：
```html
<div class="div1">这是一个div</div>
<div class="div2">这是一个div</div>
```
```css
    .div1{
        width:300px;
        margin:70px;
        border:25px;
        padding-right:60px;
        background-color:pink;
    }
    .div1{
        width:300px;
        margin:50px;
        border:25px;
        padding-right:60px;
        background-color:pink;
    }
```

一个外边距是`70px`一个外边距是`50px`没有做其他布局的情况下这两个盒子应该是上下状堆在一起的，我们看一下他们两个间距到底是多少。

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0570121c97da483888cd9b43e7b7a4b2~tplv-k3u1fbpfcp-watermark.image?)
根据这两个箭头所指，我们可以看到上方橙色全部都是第一个`div`的`margin`，下方浏览器清晰的写出了`margin`值为`70px`，也就是说，产生了**边距重叠**，并且确实合并成了**较大的那个**。

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5dce951fb16e486ab64f1dff5bb3e37d~tplv-k3u1fbpfcp-watermark.image?)
> 图画的是不是有点不忍直视emm，下次努力！！！

## BFC（边距重叠解决方案，还有IFC）解决边距重叠🍋
有些时候我们不希望他发生边距重叠，我们采用BFC和IFC来解决。

先普及一下概念，FC就是Fomatting Context。它是页面中的一块渲染区域。而且有一套渲染规则，它决定了其子元素将怎样定位。以及和其它元素的关系和相互作用.BFC和IFC都是常见的FC。
分别叫做Block Fomatting Context 和Inline Formatting Context。

**我是这样理解的：他指定了一块环境，在这块环境内部的元素布局与外界不产生相互影响**

以BFC为例，来介绍一下它的渲染规则：
- 内部盒子垂直排列，间距由margin决定
- 在同一BFC下，相邻两个盒子会发生边距重叠现象
- 计算BFC高度的时候，浮动元素也会参与计算
- BFC的区域不会与浮动的区域重叠
介绍完规则再来介绍一下如何创建BFC：
-  overflow不为visible;
-   float的值不为none；
-   position的值为absolute或fixed；
-   display属性为inline-blocks,table,table-cell,table-caption,flex,inline-flex;

规则太多，我挑个举例子吧：
```html
    <div class="left"></div>
    <div class="right"></div>
```
```css
    div {
        width:300px;
    }
    .left {
        width: 100px;
        height: 150px;
        float: left;
        background: black;
    }
    .right {
        height:200px;
        background-color:red;
    }
```

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/82da570f434448ec9d3d93be5af1532c~tplv-k3u1fbpfcp-watermark.image?)
可以看到浮动的地方与另一块内容重叠了，这个时候我们在红色区域创建一个BFC，使其不重叠。
```css
.right {
  overflow:hidden;
  height:200px;
  background-color:red;
}
```

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f36b3315339d45079259ead9d6d91078~tplv-k3u1fbpfcp-watermark.image?)

他们两个成功被分开了,证明我们的BFC区域创建成功了。
>其余的规则如果也想验证一下可以自己尝试一下~

在证明一下BFC能够解决边距重叠问题：
```html
    <div id="margin">
        <p>1</p>
        <div style="overflow: hidden">
            <p>2</p>
        </div>
        <p>3</p>
        <p>4</p>
    </div>
```
```css
    * {
        padding: 0;
        margin: 0;
    }
    #margin {
        background: #000;
        overflow: hidden;
    }
    p {
        margin-top: 15px;
        margin-bottom: 25px;
        background: red;
    }
```

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fcac2d45feff4c1cb2aa05143ba3e28d~tplv-k3u1fbpfcp-watermark.image?)
我们看2，它与1、3都没有边距重叠，这是因为它的父元素中具有`overflow:hidden`，这就创建了一个BFC使其不受外界环境影响。我们再看3、4。

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fbae654d8bf74e7ba6905c1d224e350a~tplv-k3u1fbpfcp-watermark.image?)
可以看到3的下边距与4是发生了重叠的，这是因为它不具有BFC，就如同之前一样，边距会发生重叠最终合并成较大的那一个。
## 结束啦🎉
对于CSS来讲，最主要还是布局，在布局之中`盒子模型`有有着很重要的地位，所以先弄懂它，我们一步步来🏃~

梳理好每一个知识点，稳扎稳打，才不会被面试官问倒😰~

如果文章有误欢迎在评论区指出，感谢指正🔔

这是我面试专栏的第一篇文章，后续会陆陆续续继续整理的，欢迎大家关注📢

👉我的掘金专栏地址：[Ned的面试加油站](https://juejin.cn/column/7032653725189537805)👈

如果您觉得以上的内容还不错，不妨点个赞支持一下哦~~😇

我们下期再见👋