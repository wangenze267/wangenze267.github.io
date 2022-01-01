---
title: 我的第一次PR，一个炫酷的个人介绍页面
author: Ned
tag:
  - Vue
  - 经历
abbrlink: 3328562731
---
## 前言

一直想做个关于介绍的个人页面，挂在域名的根路径下面，当home页用，还不想手动的去自己从0到1的去做一个，觉得那很浪费时间，直到我前几天逛github，发现了这个项目，瞬间觉得，它就是我想要的样子~
## 个人简介页面模板
![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6b0b33078d5040db84a282baf0c479b5~tplv-k3u1fbpfcp-watermark.image?)
女生自用99新？？？？引起了我极大的好奇心，连忙点进去。

![111.jfif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c492f3828bc443dfb1d532e1150759c7~tplv-k3u1fbpfcp-watermark.image?)

这个项目是Vue做的，READEME里面给与了充足的介绍，跟效果图，demo网址等，给了小白充足的了解去把这个项目更改成为自己的一个介绍网页，里面的功能也是十分的齐全。

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6c1a0dcbefde4e5bbcffcf2cb7b13224~tplv-k3u1fbpfcp-watermark.image?)

> 如果有需要可以直接点击传送门：[Jiaocz/Personal-page](https://github.com/Jiaocz/Personal-page)

我也是十分的欣喜，终于找到了我喜欢的样子，于是直接`git clone`，将项目拉下来，进去照着注释，文档，各种改了改，于是他最后成了这个样子：
> 传送门查看效果：[Ned](https://www.wangez.site/)

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bd47c232658c4063a6523945c9c2112e~tplv-k3u1fbpfcp-watermark.image?)

这里我又要说一件事情了，这个项目原来，下方的icon不是我截屏中的这样，而是这个效果：

![原来.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/adc7704ce85044ea835363ccc9e133c6~tplv-k3u1fbpfcp-watermark.image?)

而我心中的页面，下方想要放一些自己的站点，点击可以直接跳转的那种，并且我觉得他原来做的动效也很好看，可以保留，于是就想着，我能不能自己给他改动一下，使得🐟和🐻可以兼得呢？

> 是不是做出来还可以提个PR，满足我的开源梦~

## 我的第一次PR
说做就做，我去找到了有关icon的函数，查了一下相关的配置，发现我这个需求其实不难实现。
用到的原本的数据格式是这样：
```json
[
    {
      name: "Github",
      icon: "icon brands fa-github",
      desc: "Github",
      content: "https://www.wangez.site/",
      show: false,
      // jump: true,
    }
]
```
下面函数是获知用户点击了哪个icon，传入对应的name，从而去将show改成true，将其内部的content显示出来。
```js
showContact: function (name) {
    let num = 0;
    for (let i = 0; i < this.contacts.length; i++) {
      if (this.contacts[i].name == name) {
        num = i;
        break;
      }
    }
    this.hideContact(false);
    this.defaults = 0;
    clearTimeout(this.timer);
    this.timer = setTimeout(() => {
      this.contacts[num].show = true;
      this.backButton = true;
    }, 500);
}
```

我在这个数据中加入一个jump字段，也是有着`true`，`false`，两个值，如果根据name得到的那组数据中的jump字段是true，就将其跳转，false我们就不管，继续执行他原本的动效操作。
```js
showContact: function (name) {
    let num = 0;
    for (let i = 0; i < this.contacts.length; i++) {
      if (this.contacts[i].name == name) {
        num = i;
        break;
      }
    }
    if (this.contacts[num].jump == true) {
      window.open(this.contacts[num].content);
      return;
    }
    this.hideContact(false);
    this.defaults = 0;
    clearTimeout(this.timer);
    this.timer = setTimeout(() => {
      this.contacts[num].show = true;
      this.backButton = true;
    }, 500);
}
```
改完，测试完的我，就兴致勃勃的提交了我人生中的第一个PR，后来跟项目作者在github上交流了几次，也做了部分修改后，我的PR成功被采纳了！！！！！

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c9f6bc5979df4a659b1d38a45e5e12e3~tplv-k3u1fbpfcp-watermark.image?)

虽然改动的地方不多，可能还不够完善，但是今后我会持续发光发热，争取把我的想法输出在这个项目中，也是给那些像我一样，懒的自己做，只想找一个自定义更丰富的个人页面的人一个满意的结果。
## 至于我的页面
我的icon跟作者使用的不同，使用的阿里矢量库中的icon，所以我将其中的a标签换成了span，并更改了一些样式，可能后续的我会把我下方icon的部分也提交到仓库中，供给更多的朋友们使用。
## 最后
第一次成为Contributor呀~，十分的激动hhhhh

给以后的自己，加个油吧，要更加努力！

本文首发于[掘金 ](https://juejin.cn/user/105972016875911)。