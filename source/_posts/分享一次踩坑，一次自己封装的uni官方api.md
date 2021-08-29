---
title: 分享一次踩坑，一次自己封装的uni官方api
author: Ned
tag:
  - 踩坑日记
  - uni-app
abbrlink: 1196783753
---



### 前言

这几天在做一个app，用来打比赛用，使用的是uni+uView的组件库。这个组件库是半道加进来的，学弟推荐的，我看有组件的话确实会方便很多，而且他都是按需引入，不占用额外空间，挺好的，我也就直接拿来用了。

使用感想：感觉这套技术栈跟vue+element差不多了emm，就是某些官方api还不太一样，还是比较顺手的。

组件库传送门：[ uView - 多平台快速开发的UI框架 ](https://www.uviewui.com/components/intro.html)

但是，不熟悉组件的api以及参数，让我踩了坑，非常难受emm，还是写个记录，长长记性。

本次内容：分享一个踩坑，一个自己进行封装的uni官方api

> 毕竟踩坑日记专栏好久没更新了。

<!--more-->

### 坑：uView组件库，uToast组件

传送门：[Toast 消息提示 ](https://www.uviewui.com/components/toast.html)

使用方法：

```js
// 将这个标签放在页面中
<u-toast ref="uToast" />
// 需要消息提示的话，加上如下代码
this.$refs.uToast.show({
    title: '登录成功',
    type: 'success',
    url: '/pages/user/index'
})
```

参数：

- `title`:消息提示显示的文本

- `type`:主题类型，不填默认为`default`（一共有6中主题色）

- `url`:toast结束跳转的url，不填不跳转

> 更多详细api请点击上方toast传送门去到该组件库官网看

我遇到的坑，便是填写了url，他执行结束后不跳转。

说一下我的大致使用情况：

登录成功后弹出消息提示登录成功，完后跳转到一个tab页面上去。但是他只给弹出登录成功，不报错也不跳转。当时真的人傻了，找了好多地方，也打了断点，所有的事情都告诉我一切正常。于是我开始怀疑起我的电脑emm，我将整个程序打包，发给了我的学弟，让他在他电脑上帮我调试一下，结果与我电脑一致（nice！那就是电脑没坏，还能用），在我吃完饭回到电脑前准备跟这个问题决一死战的时候，学弟发来的一张截图让我人呆滞了。

| **参数** | **说明**                                   | 类型    | 默认值 | 可选值 |
| -------- | ------------------------------------------ | ------- | ------ | ------ |
| isTab    | toast结束后，跳转tab页面时需要配置为`true` | Boolean | fasle  | true   |

没错，就是这个表格。我才知道这个组件要是跳转到tab页面的话还需要额外配置一个参数，赶忙加上，于是我又从坑中站了起来。

#### 关于toast结束跳转URL

- 如果配置了`url`参数，在toast结束的时候，就会用`uni.navigateTo`(默认)或者`uni.switchTab`(需另外设置`isTab`为`true`)
- 如果配置了`params`参数，就会在跳转时自动在URL后面拼接上这些参数，具体用法如下：

```js
this.$refs.uToast.show({
	title: '操作成功',
	url: '/pages/user/index',
	params: {
		id: 1,
		menu: 3
	}
})
```

这个故事告诉我们，大家使用之前一定要摸清楚api，不要学习泽泽！！！

### 封装uni.getStorage函数

这个应该不算坑，我只是觉得他这个使用起来不是很方便（好像是相当不方便）

传送门：[uni.getStorage ](https://uniapp.dcloud.io/api/storage/storage?id=getstorage)

它的作用就是从本地缓存中异步获取指定 key 对应的内容。

我的需求就是要从缓存中获取一些我之前存入进去的东西，但是我取一个还好，某一处需要取两个的话要把这个函数写两次，反正我本人是觉得很麻烦的（即使CV不麻烦，但在程序员眼中，代码相似程度有点高），那为什么不尝试着封装一下呢？

函数本来的样子：

```js
uni.getStorage({
    key: 'storage_key',
    success: function (res) {
        console.log(res.data);
    }
});
```

他是根据`key`，去取缓存中的数据，完后返回值`res.data`中就是我们需要的数据了。

思路就是：传一个`key`给他，同时接好`res.data`的值

改造前：

```js
uni.getStorage({
    key: 'token',
    success:(res) => {
        console.log('token:'+ res.data)
        this.token = res.data
    }
})
uni.getStorage({
    key: 'username',
    success:(res) => {
        console.log('username' + res.data)
        this.userInfo.username = res.data
    }
})
```

改造后：

```js
getCache(a){
    let b = '';
    console.log('a:'+a)
    uni.getStorage({
        key:a,
        success:(res)=>{
            console.log('res.data:'+res.data);
            b = res.data;
            return res.data;
        }
    })
    return b;
},
```

需要调用的时候这样：

```js
let username = this.getCache('username')
```

是不是非常的方便呢？

> 封装，yyds

### 最后

踩坑可以迟到，但永远不会缺席，我们能做的就是每次细心一点，并且把犯过的错，背过的锅记录在小本本上，再也不去犯第二次。

毕竟，神也会犯错呀！

祝愿天下的程序员都少一点bug吧。

### 改进上面的封装函数

自己今天使用的时候发现，这样封装只能单页面内使用，可是一个项目中，我有好几处都要使用，就只能复制粘贴。

> 那我不又成了cv程序员？？？

于是我继续开始封装，想把它以组件的方法进行封装。

`cache.js`，我给他放在了common文件夹内

```js
 export function getCache(a){
	let b = '';
	console.log('a:'+a)
	uni.getStorage({
		key:a,
		success:(res)=>{
			console.log('res.data:'+res.data);
			b = res.data;
			return res.data;
		}
	})
	return b;
}

```

**使用方法：**

在需要使用这个函数的页面进行引入就好，方法类似于我们按需引入组件。

```js
import {getCache} from 'common/cache.js'
```

使用的时候直接进行调用就好。

```js
this.username = getCache('username')
```

