---
title: 前端也要学习基本的UI设计原则与实践套路
author: Ned
tag:
 - 经历
 - 字节青训营
abbrlink: 2086971120
---



## 前端也要学习基本的UI设计原则与实践套路

### 前言

有的人可能说，我是技术研发人员，UI的事情我们团队内会有别的人去操心这个，我只管技术即可。

在一天之前，我也是这么觉得的，但是我今天听了字节的《给开发看的UI设计》这节课后，觉得一个前端工程师也是要具备一定的UI设计能力的。

依赖市面上的组件库已经不能让产品维持在好用的状态了，还需要将一些设计元素添加进去，才能让我们开发的作品，达到一个更好的层次，给与用户最好的体验。

你的团队可能没有UI同学，也可能有UI同学，但是不一定专业，他们经常会是外包人员，UI给出的设计稿通常只是静态文件，是某一交互切面的，很多的交互细节都体现不出来，在大厂中，许多的B端产品是没有专职UI角色的，前端可能要对产品最终呈现出的形态负责。

<!--more-->

### **设计中最重要的事情------交付功能**

工具：[最好用的白板------excaildraw](https://excalidraw.com/)

我们只需要简单粗略的设计大框就可以，设计细节可以后面在补充，在设计UI的时候永远不要忘了，我们归根结底还是要以实现需求，功能优先。

> 可以使用别的设计工具去画原型图（figma），但是我觉得作为开发人员，简简单单的就可以，一切以方便为主

要设计简单的、完整的功能

以MVP版本功能来作为设计目标

- MVP：Minimum Viable Product，最简化可实行产品

所谓先让游戏开始，我们开发在做设计的时候，优先把主要场景设计完成，有迅速迭代能力就可以了，正所谓不需要太多花里胡哨，保持结构简单，功能完整就好。

### 设计原则

**设计原则------层级**

层级------是我们可能唯一要关心的原则

层级区分，更是我们最需要掌握的原则和技巧。

- 一个产品要好用，就要让用户很容易的抓住展品重点
- 在重点里，用户能自在地进入沉浸式阅读，和使用的转台
- 当用户想探索其他内容时，ta能轻松的识别网站的次要板块还有哪些，焦点能迅速转义，并迅速沉浸
- 辅助提示信息，精确而又不会打扰各主要板块的呈现

**设计原则------一致性**

用户在站点的各个角落，观察到的颜色、间距、阴影、位置、字体和字重的运用，都在一套有限的框架里，一套能迅速建立起亲切感的框架内。

> 当页面中，主要的交互、视觉元素都采用同一主题色来表示时，用户可以迅速知道：
>
> - 页面中哪些元素的可以交互的
> - 我需要的提示信息在哪

**设计原则------《写给大家看的设计书》**

简单介绍一下《写给大家看的设计书》四原则：

- 对比：如果两个元素内涵不同，请让它们截然不同
- 重复：设计的视觉要素应当在整个作品中重复出现
- 亲密性：彼此关联的元素应当靠近放置在一起
- 对齐：任何元素都不能随意安放，应当总与另外至少一个 元素有视觉上的关联

这四个原则其实就对应着上面的一致性和层级。

- 层级：就是亲密性+对比的目标，让用户抓重点、切视线，又快又稳。
- 一致性：就是对齐+重复，克制用户实现所感受的尺度，迅速与网站设计语言建立熟悉感。

### 设计体系

**设计体系------布局**

最基本的布局技巧，内容居中放。

多列布局：

- 主要内容弹性收缩（可以有最小宽度），次要列固定宽度

**设计体系------间距**

保证元素间有基本的间距，是最基本的设计技巧。

间距------多留白

这是一个技巧：安排元素时先大大的留空，要从松到紧的调试间距。

> 间距是远比色块、边框、分界线之类的更适合用来表达关联关系的工具。值得多加练习运用。

**设计体系------文字**

文本是站点的主要内容载体，字体的设计自然也是重中之重。

**设计体系------色彩**

谁人不喜色彩？

你选择的色彩库要有10种左右的灰色度供你使用（黑-->白），要有主题色，功能色等。

要学会使用色盘，来微调，使得你的颜色对用户使用的影响更好

**注意事项：**

- 颜色虽好，使用不当会打破页面层级平衡
- 色盲消费不了颜色
- 颜色在不同文化中可能有不同的含义

**设计体系------深度**

深度的感官来源于生活，类似于光照的效果，来打造一种层级。

> 就是我们常用的阴影感

### 实用技巧

**实用技巧------图片上的层级**

图片上的色块斑驳不一，难以找到合适的前景色。咋办？

- 增加蒙层，对比度不就上来了。
- 可以给文字加上阴影，增加文字部分的对比度，不影响整张图片。

**实用技巧------明晰的用户头像**

上传头像的场景，如何清晰的展示内容？

- 加点内阴影

**实用技巧------干掉分界线**

阴影可以替代边框，不通的背景色块也可以。

间距清晰的话，也就不需要分界线了。

### 最后

听完了这节课真的觉得挺有用的，虽然说讲的是设计理念，没有一丝css代码在其中，可是我觉得我的css能力上升了一个层面~

最后留下这份笔记，留给以后的自己进行梳理，也顺便分享出来给更多的前端小伙伴们，让更多之前与我一样的人了解到，前端也要懂UI。