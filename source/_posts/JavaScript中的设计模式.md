---
title: JavaScript中的设计模式-代理模式
author: Ned
tag:
  - JavaScript
  - 设计模式
abbrlink: 1278378512
---

### 前言

学学设计模式，每次更一个，实在太多了。

第一次更新：**单例模式**

第二次更新：**策略模式**

第三次更新：**代理模式**

> 打算10月前将JavaScript滚个百分之六七十，不知道能不能学完。
>
> 计网操作系统数据结构算法还没学
>
> vue还没盘
>
> 我还有机会吗

<!--more-->

### 什么是设计模式？

在软件设计过程中，针对特定问题的简洁而优雅的解决方案。

把之前的经验总结并且合理运用到某处场景上，能够解决实际的问题。

### 设计模式五大设计原则（SOLID）

- S-单一职责原则

  > 即一个程序只做好一件事

- O-开放封闭原则

  > 可扩展开放，对修改封闭

- L-里氏置换原则

  > 子类能覆盖父类，并能出现在父类出现的地方

- I-接口独立原则

  > 保持接口的单一独立

- D-依赖导致原则

  > 使用方法只关注接口而不关注具体类的实现

### 为什么需要设计模式？

- 易读性

  > 使用设计模式能够提升我们的代码可读性，提升后续开发效率

- 可拓展性

  > 使用设计模式对代码解耦，能很好的增强代码的yi修改性和拓展性

- 复用性

  > 使用设计模式可以复用已有的解决方案，无需重复相同工作

- 可靠性

  > 使用设计模式能够增加系统的健壮性，使代码编写真正工程化

### 常见设计模式

#### 单例模式

定义：**唯一&全局访问。保证一个类仅有一个实例，并提供一个访问它的全局访问点。**

> 另外一种多例模式，通过一个类构造出多个不一样的实例，这就是多例模式。
>
> 单例模式与多例模式最本质的区别：实例的数量。
>
> 单例模式永远只有一个实例，这个实例可以被缓存起来，可以复用。

应用场景：就是能被缓存的内容，例如登录弹窗。

> 我觉得就是一个地方如果在你的项目中可以用到两次或两次以上，都可以尝试一下这个，能够减少很多代码。

来看这段伪代码：

```js
const creatLoginLayer = () => {
    const div = document.createElement("div");
    div.innerHtml = "登录浮窗";
    div.style.display = "none";
    document.body.appendChild(div);
    return div;
};

document.getElementById("loginBtn").onclick = () => {
    const loginLayer = creatLoginLayer();
    loginLayer.style.display = "block";
};
```

`creatLoginLayer`的作用是创建一个登录浮窗并将节点添加到body上，下面做的是登录按钮的一个点击事件，点击登录按钮就会创建登录浮窗并将`display`从`none`改为`block`，将他显示出来。

这个逻辑是没毛病的，但是我们想一下，每点击一下登录按钮就要执行这些代码，一个项目中如果有很多地方要呢？我们上面这短短几行而已，如果是上百上千甚至上万呢？是不是就非常损耗性能，这个时候，我们的单例模式就派上了用场。

使用单例模式后：

```js
const getSingle = (fn) => {
    let result;
    return (...rest) =>{
        return result || (result = fn.apply(this.rest));
    };
};

const creatLoginLayer = () => {
    const div = document.createElement("div");
    div.innerHtml = "登录浮窗";
    div.style.display = "none";
    document.body.appendChild(div);
    return div;
};

const createSingleLoginLayer = getSingle(createLoginLayer);

document.getElementById("loginBtn").onclick = () => {
    const loginLayer = createSingleLoginLayer();
    loginLayer.style.display = "block";
};
```

可以见到，增加了一个`getSingle`函数，这里有个闭包的概念，result变量只要一直在引用就不会被销毁，起到了一个缓存的作用，函数的参数是一个`function`，如果result是null或者undefined就执行后面的逻辑，将这个传进来的函数的返回值也就是这个`div`赋给result，这样我们下面的函数就执行一次就可以了，下次调用的时候result有值，所以就直接返回了，不会在执行后面的逻辑。

#### 策略模式

定义：**定义一系列的算法，把它们一个个的封装起来，并且使它们可以相互转换。把看似毫无联系的代码提取封装、复用，使之更容易被理解和拓展**

> 它就像我们解决问题的思路，有很多种，那我们就应该选择最适合解决当前业务的那一种。

应用场景：要完成一件事情，有不同的策略。例如绩效计算、表单验证规则。

来看这段代码：

```js
const calculateBonus = (level, salary) => {
    switch (level) {
        case 's': {
            return salary * 4;
        }
        case 'a': {
            return salary * 3;
        }
        case 'b': {
            return salary * 2;
        }
        default: {
            return 0
        }
    }
    return strategies[level][salary];
};
calculateBonus['s',20000];
```

函数`calculateBonus`接收level跟salary两个参数，运用switch case来计算绩效。但是在以后我们又要增加新绩效的时候，例如我们想加一个p绩效，我们要深入到代码中，去在switch case中去增加一个 case p的逻辑，这样子我们相当于每次业务变更都要去改造这个函数，就是不太好的情况。

> 使用switch还好，代码看起来很清晰，如果是if else呢   不敢想象了~

再看策略模式：

```js
const strategies = {
    s: (salary) => {
        return salary * 4;
    },
    a: (salary) => {
        return salary * 3;
    },
    b: (salary) => {
        return salary * 2;
    },
};

const caculateBonus = (level, salary) => {
    return strategies[level][salary];
};
caculateBonus('s',20000)
```

也是有着一个`caculateBonus`函数，接收level跟salary两个参数，但是我们构建了一个策略表，在策略表中维护这个计算规则。这时候如果要加入一个新绩效等级，就去表中加入一个新等级，再加入它的计算规则即可，并且可以将表提取出来放到另一个文件中，甚至是放到公网上下发，因为这个表是可以不用程序员们来维护的，可以做成页面交给公司其他人员去维护，就有了前面说的那个下发的功能，可以去给别人展示。计算绩效的时候去读取策略表，完后根据表中规则来进行计算就行。

#### 代理模式

定义：**为一个对象提供一个代用品或者占位符，以便控制对它的访问。替身对象可对请求预先进行处理，再决定是否转交给本体对象。**

> 假如项目中一个图片过大，短时间内加载不出来的时候，用户只能看见白屏，体验感就会非常糟糕，这时候我们就可以用一个替身来暂时替代图片。
>
> 当真身被访问的时候，我们先访问替身对象，让替身代替真身去做一些事情，之后在转交给真身处理。	 

应用场景：当我们不方便直接访问某个对象时，或不满足需求时，可考虑使用一个替身对象来控制该对象的访问。

来看这段代码：

```js
// 原生函数
const rawImage = (() => {
	const imgNode = document.createElement("img");
	document.body.appendChild(imgNode);
	return {
		setSrc:(src)=> {
			imgNode.src = "./loading.gif";
			const img = new Image();
			img.src = src;
			img.onload = () => {
				imgNode.src = this.src;
			}
		}
	}
})();

rawImage.setSrc("http://xxx.gif");
```

这个xxx.gif的加载时间可能要到10s左右，我们首先预加载了一个loading图，用户看到的过程是最开始看见loading，当xxx加载完成后，也就是onload执行完后，就会做一个替换的效果，完成了这个功能。

思考：这段代码是不是耦合性太强了啊？

是的，loading的逻辑跟我们实际加载的逻辑是耦合在一起的，我们要做两个事情，一个是设置loading，一个是设置图片链接。那我们给他分开，用代理函数去做预处理，加载loading，用原生函数提供一个设置图片链接的功能。

实现一下：

```js
// 原生函数
const rawImage = (() => {
	const imgNode = document.createElement("img");
	document.body.appendChild(imgNode);
	return {
		setSrc:(src)=> {
			imgNode.src = src;
		},
	};
})();

// 代理函数
const proxyImage = (() => {
	const img = new Image();
	img.onload = () =>{
		rawImage.setSrc(this.src);
	};
	return {
		setSrc:(src) => {
			rawImage.setSrc("./loading.gif");
			img.src = src;
		},
	};
})();

proxyImage.setSrc("http://xxx.gif");
```

我们通常会进行一些请求的预处理的时候使用代理模式。

#### 发布订阅模式

#### 命令模式

#### 组合模式

#### 装饰器模式

#### 适配器模式

