---
title: 弹性盒模型-flex
author: Ned
tags:
  - Css
  - Flex
categories:
 - Css
abbrlink: 3659327966
---

#### 前言

最开始接触flex布局的时候，还是在阮一峰老师的博客上学习的，最近在看一本书，上面有提及到，于是就来整理一下所学，有理解的不对的地方还请指正。

### 弹性盒模型

#### 简介

弹性盒模型和盒模型的概念有一些区别，他不在是定义元素的，而是CSS3引入的一种布局方式，其核心思想是调整元素大小来适应不同空间，包含垂直方向的高度和水平方向的宽度。

<!-- more -->

弹性盒模型有两个重要的概念：容器（container）和项目（item）。其中容器属性用来设置项目的公共样式，比如排布方向、排布对齐方式；项目属性用来设置各个项目之间的相对关系，包括比例、顺序等。容器和项目是嵌套关系，一个容器可以有多个项目，其中容器的display的属性值应设为flex。

标准盒模型的排布包括水平和垂直方向，弹性盒模型也是如此。我们可以将水平方向理解为主轴，垂直方向理解为交叉轴，与传统盒模型有所不同的是，弹性和模型在排列方向上除了从左到右和从上到下，还支持反序排列，即从右到左和从下到上。

容器的属性有6个，我们下面一一介绍他们：

##### 1.justify-content

定义主轴上项目的对齐方式。

- flex-start:默认值。项目靠近主轴首端的方向排列。
- flex-end:项目靠近主轴末端首端的方向排列。
- center:项目靠近主轴的中间位置排列。
- space-between:两端对齐。第一个项目会紧贴主轴首端，最后一个项目会紧贴主轴末端，其余项目等间距排列。
- space-around:每个项目外边距相等，项目之间的间距是项目与边框间距的两倍。
- space-evenly:可以使得相邻项目、项目与边框之间的间距相等。

##### 2.align-items

定义多个项目单行排列下的交叉轴对齐方式。

- flex-start:项目靠近交叉轴首端的方向。
- flex-end:项目靠近交叉轴末端的方向。
- center:项目与交叉轴中线对齐。
- baseline:项目与第一行文字基线对齐。
- stretch:将项目拉伸至与交叉轴两端对齐。

##### 3.align-content

定义多个项目多行情况下在交叉轴的对齐方式，和主轴一样，也支持六中队旗方式，只是个别的名称有所变化。

- flex-start:项目靠近交叉轴首端的方向排列。
- flex-end:项目靠近交叉轴末端的方向排列。
- center:项目靠近交叉轴的中间位置排列。
- space-between:两端对齐。第一个项目会紧贴交叉轴首端，最后一个项目会紧贴交叉轴末端，其余项目等间距排列。
- space-around:每个项目外边距相等，项目之间的间距是项目与边框间距的两倍。
- stretch:可以使得相邻箱门、项目与边框之间的间距相等。

##### 4.flex-direction

定义容器主轴上项目的排列方向。

- row:默认值，水平方向从左往右排列。
- row-reverse:水平方向从右往左排列。
- column:垂直方向从上往下排列。
- column-reverse:垂直方向从下往上排列。

##### 5.flex-wrap

用来控制项目在主轴上是否换行。

- nowrap:默认值，项目超出容器尺寸时不换行。这个尺寸根据排列方向而定，对于水平排列方式为宽度，垂直排列方式为高度。
- wrap:项目超出容器尺寸时换行。
- wrap-reverse:项目超出容器尺寸时换行，不过是反序排列。例如水平方向布局的话先从下面一行开始往上换行。

##### 6.flex-flow

这个属性是一个简写属性，其第一个属性值用来设置flex-direction的值，第二个属性值用来设置flex-wrap的值，两个值用空格做分隔符。默认值为“row nowrap”。
如果需要对不同项目进行定制化设置，那么还需要用到项目属性。项目属性可分为三类，一类和项目顺序有关(order)，另一类和项目对齐方式有关(align-self)，最后一类和项目的尺寸相关（flex-grow,flex-shrink-flex-basis,flex）,具体介绍如下。

##### 7.order

它的值是一个整数，按照数值从大到小的顺序排列项目。默认值为零，即按照元素编写顺序排列。

##### 8.align-self

可以覆盖容器定义的align-items属性，为当前项目单独设置交叉轴的对齐方式，可选值同align-items。

##### 9.flex-grow

定义项目在容器拥有剩余空间时的放大比例。默认值为零，如果各个项目的值都相等，那么均分剩余空间，否则按照flex-grow定义的比例分配剩余空间。

##### 10.flex-shrink

定义项目在容器空间不足时的缩小比例。默认值为1，如果各个项目的值都相等，那么均匀缩小各个空间，否则按照flex-shrink定义的比例进行缩小。

##### 11.flex-basis

定义项目的默认尺寸，可选值同width属性，默认值为auto。

#### 基础布局

##### 1.单列布局

```
<!-- html -->
<div class="container">
  <div class="item">main</div>
</div>
/*css*/
.container {
  display:flex;
  background-color:lightgrey;
  justfy-content:center;
}
.item {
  flex-basis:1000px;
  background-color:yellow;
}
```

思路很简单，设置容器主轴项目对齐方式居中，同时设置好项目宽度即可。

##### 2.两列布局

```
<!-- html -->
<div class="container">
  <div class="center">center</div>
  <div class="left">left</div>
</div>
/*css*/
.container {
  display:flex;
}
.center {
  flex-grow:1;
  background-color:yellow;
  order:1;
}
.left {
  background-color:red;
  flex-basis:200px;
}
```

两列布局的要点就是设置两个项目的宽度，其中左侧项目通过flex-basis属性设置为固定宽度，另一个项目将flex-grow设置为1，让其根据剩余空间自动撑满。为了让右侧元素先加载，可以使用order属性调整其位置。

##### 3.三列布局

三列布局的处理思路和两列布局是一致的，这里就不多做阐述，都是利用项目之间的比例关系来实现。