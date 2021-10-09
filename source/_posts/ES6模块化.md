---
title: 快速了解ES6模块化少不了这篇文章
author: Ned
tags:
  - ES6
abbrlink: 3892826279
---

## 前言

在之前的JavaScript中是没有模块化概念的，无法将一个大程序拆分成互相依赖的小文件，再用简单的方法拼装起来。如果要进行模块化操作，就需要引入第三方的类库。随着技术的发展，前后端分离，前端的业务变的越来越复杂化，于是才有了ES6模块化的诞生。

为什么要有模块化，或者模块化的好处是什么呢？

>  大家都遵守同样的模块化规范写代码，降低了沟通的成本，极大方便了各个模块间的相互调用，利人利己。
>
>  可以将一段复杂的程序拆解开来，方便维护可拓展。

<!--more-->

## 前端模块化规范

在**ES6模块化**诞生之前，JavaScript社区尝试并提出了**AMD、CMD、commonJS**等模块化规范。

但是，这些模块化规范，存在一定的差异性与局限性，并不能通用。

例如：

- AMD和CMD适用于浏览器端的JavaScript模块化

- commonJS适用于服务器端的JavaScript模块化

  > Node.js 就是遵循的这个规范
  >
  > 导入其它模块使用require()
  >
  > 导出使用module.exports对象

太多的模块化规范给开发者增加了学习的难度与开发的成本。所以，ES6模块化规范诞生了！

### 什么是ES6模块化规范

ES6模块化规范是浏览器端与服务端通用的模块化开发规范。它的出现极大的降低了前端开发者的模块化学习成本，开发者不需要在额外学习AMD、CMD或者commonJS等模块化规范。

ES6中模块化规范中定义：

- 每个js文件都是一个独立的模块
- 导入其他模块成员使用`import`关键字
- 向外共享模块成员使用`export`关键字

## 在node.js中体验ES6模块化

node.js中默认仅支持commonJS模块化规范，若想在node中进行体验，要按照如下两步骤进行配置：

- 确保安装了`v14.15.1`或者更高版本的node.js

  > 可以使用在cmd窗口中使用`node -v`命令查看当前版本号哦~

- 在package.json的根节点中添加`"type":"module"`节点

  > 不知道如何添加的小伙伴看这里：
  >
  > 首先我们要在一个空文件夹内，执行`npm init -y`，这时候我们就能看见已经自动生成了`package.json`文件了
  >
  > 完后在vs-code打开，在内添加`"type":"module"`节点即可
  >
  > 小提示：type值默认为commonJS，所以我们平时node遵循的模块化规范都是commonJS

## ES6模块化的基本语法

ES6的模块化主要包含如下3种用法：

- 默认导出与默认导入
- 按需导出与按需导入
- 直接导入并执行模块中的代码

### 默认导出

语法：`export default` <font color="nred">默认导出的成员</font>

```js
let n1 = 10 // 定义模块私有成员 n1
let n2 = 20 // 定义模块私有成员 n2 因为没有共享出去，所以外界访问不到
function show(){  // 定义模块私有方法 show
	
}
export default { // 使用export default 默认导出语法 向外共享n1 和 show 两个成员
	n1,
    show
}
```

**注意事项**

每个模块中，只允许用唯一的一次 `export default`，否则会报错！

### 默认导入

语法：`import`<font color="nred">接收名称</font>`form`<font color="nred">模块标识符</font>

```js
// 从 m1.js 模块中导入 export default 向外共享的成员
// 并使用 m1 进行接收
import m1 form './m1.js'

console.log(m1)
// 输出为: { n1: 10, show: [Function:show]}
```

**注意事项**

默认导入的时候，接收名字可以任意写，注意是合法的成员名称就行。

```js
// m1 合法 不报错
import m1 form './m1.js'
// 成员名称不能用数字开头，所以会直接报错
import 123 form './m1.js'
```

### 按需导出

语法：`export`<font color="nred">按需导出的成员</font>

```js
// 向外按需导出变量 s
export let s = 'Ned'
// 向外按需导出方法 show
export function show(){}
```

### 按需导入

语法：`import {s} from`<font color="nred">模块标识符 </font>

```js
import { s, show } form './m1.js'
console.log(s) // Ned
console.log(show) // [Function: show]
```

**注意事项**

- 每个模块中可以使用多次按需导出
- 按需导入的成员名称必须跟按需导出的名称一致
- 按需导入时，可以使用`as`关键字进行重命名
- 按需导入可以和默认导入一起使用

重命名：

```js
import { s as str } form './m1.js'
```

使用as关键字，将s重命名为str，所以接下来我们使用str就好了，不能再使用s这个名字。

按需导入和默认导入一起使用：

```js
import info,{ s as str } form './m1.js'
```

info就是默认导入，后面带大括号的就是按需导入。

### 直接导入并执行模块中的代码

如果只想单纯的执行某个模块中的代码，并不需要得到其内部向外共享的成员，可以这样做：

```js
// m1.js:
for(let i = 0; i < 10; i++){
	console.log(i)
}
-------------------------
// 直接导入并执行模块中的代码
import './m1.js'
```

没错，就是直接导入即可。

## 最后

这篇文章简单介绍一下模块化的概念和语法，过几天我还会出一篇文章来告诉大家模块化在实际应用里是如何使用的。
