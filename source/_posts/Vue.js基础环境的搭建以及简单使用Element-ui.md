---
title: Vue.js基础环境的搭建以及简单使用Element-ui
author: Ned
tags: Vue
categories:
 - Vue
abbrlink: 3896443464
---

# Vue（2.x）

## Vue的环境配置

### Node.js的安装

Vue的运行是依赖node进行的，所以安装Vue之前，要先去node的官网去安装node。安装结束之后可以使用下面的命令查看安装是否成功。

```
node -v
```

### npm/yarn

npm（node package manager），俗称包管理器（类比于java里的maven），顾名思义，主要功能就是管理node包，包括：安装、卸载、更新等等。

他未经更改过的源，是国外的镜像，所以经常会有包丢失或者下载速度慢，建议自己换成淘宝源（阿里做的同步镜像）

<!-- more -->

```
//临时
npm --registry https://registry.npm.taobao.org install express
//永久
npm config set registry https://registry.npm.taobao.org
npm install express
```

yarn的话，直接去官网下载安装就行。

### Vue-cli

```
npm install vue-cli -g
```

Vue CLI 是一个基于 Vue.js 进行快速开发的完整系统，提供：

- 通过 `@vue/cli` 实现的交互式的项目脚手架。

- 通过 `@vue/cli` + `@vue/cli-service-global` 实现的零配置原型开发。

- 一个运行时依赖 (

  ```
  @vue/cli-service
  ```

  )，该依赖：

  - 可升级！；
  - 基于 webpack 构建，并带有合理的默认配置！；
  - 可以通过项目内的配置文件进行配置！；
  - 可以通过插件进行扩展！。

- 一个丰富的官方插件集合，集成了前端生态中最好的工具。

- 一套完全图形化的创建和管理 Vue.js 项目的用户界面！ 。

Vue CLI 致力于将 Vue 生态中的工具基础标准化。它确保了各种构建工具能够基于智能的默认配置即可平稳衔接，这样你可以专注在撰写应用上，而不必花好几天去纠结配置的问题。



到现在，Vue的环境可以说是暂时配置完成了！下面我们去new一个Vue项目吧。

## Vue项目的搭建（Vue-ui）

首先打开vs-code，在终端中输入：

```
vue ui
```

![vueui](https://wangez.site/img/shangke/vueui.png)

等待一会，就会打开Vue的一个图形化管理界面。

![vue-ui](https://wangez.site/img/shangke/vue-ui.png)

点击左下角那个小房子的按钮，进入到项目管理的主页

点击创建项目。

![chuangjian](https://wangez.site/img/shangke/chuangjian.png)

初始化git仓建议勾选打开，包管理器默认就好，接着我们只需要输入项目名字就可以点击下一步了

![detail](https://wangez.site/img/shangke/detail.png)

他会让我们选择一套预设，图中的第一个，是Vue2的官方默认模板，第二个是Vue3的默认模板，第三个选项是让我们手动配置本项目的配置，并可以保存成模板，方便下次使用，最后一个就是从你的git仓库去拉取一套预设，作为本项目的预设。

因为方便大家操作，所以我们选择手动配置。

![gongneng2](https://wangez.site/img/shangke/gongneng1.png)

![gongneng2](https://wangez.site/img/shangke/gongneng2.png)

来详细说明一下这些选择都是什么意思。

- 选择Vue版本，这个就是让你选择你项目到底是用Vue2还是Vue3来做开发
- Babel，是一个JavaScript编译器。
- TypeScript（微软开发），JavaScript的超集，可以编译成JavaScript。
- Router，vue-router，路由组件
- Vuex，提供vue中的状态管理
- CSS Pre-processors，CSS预处理，Less，Sass，Stylus。
- Linter/Formatter，代码质量检查，代码规范验证。（ESlint真的很烦人）
- 使用配置文件，这个是中文的，我就不解释了，勾选就行！

我勾选的有1、2、router、Linter/Formatter、使用配置文件。（因为Vuex，在本次演示中用不到）

进入配置页面，

![peizhi](https://wangez.site/img/shangke/peizhi.png)

第一个，我们看见后面2.x的时候就明白了，这个就是选择Vue2 or Vue3的一个选项。

第二个，日常关闭就好，对我们作用不大。

重点来了，第三个是让我们选择代码检查的一个严格程度。

提供了四个选项：

- ESlint with error prevention only (友好)
- ESlint + Airbnb config (地狱)
- ESlint + Standard config (标准)
- ESlint + Prettier (美观？)

我标注的备注都很明显的说出来了这几个标准是什么样子的，本人选的是第一个，但是不管选哪个都无所谓，这个可以进去从配置文件进行修改，只是有点麻烦的事情。

![baocunyushe](https://wangez.site/img/shangke/baocunyushe.png)

点击创建，他会提示你是否保存此次预设，可以依据自己的情况来选择。

这样，我们的项目就创建完成了！

### Hello Vue

打开你的开发工具（我使用的是Vs code）

打开该项目的终端

输入

```
npm run serve 或者 yarn serve
```

![yunxing](https://wangez.site/img/shangke/yunxing.png)

第一个是本地地址

第二个是网络地址

我们在浏览器访问一下：

![chuagnjiantrue](https://wangez.site/img/shangke/chuagnjiantrue.png)

至此，一个Vue项目就创建成功了。

## Vue的目录结构

- node_moudules:依赖保存的地方
- public:静态资源保存的地方
- dist:项目打包过后，需要挂载的目录
- src:写代码的地方
  - assets：src中引用的静态资源（要区别于public static）
  - componets：组件
  - router：路由
  - views：视图（页面）
  - App.vue：页面共有
  - main.js：全局依赖或者配置
- package.json：配置项目的基本信息和依赖的大概版本，这里强调一下，该文件的依赖版本只是指定大概的范围，具体真是下载的版本以package-lock.json为主
- package-lock.json：记录node_modules依赖的真是版本
- README.md:项目说明

##  Element-ui

element-ui是饿了么团队基于Vue2，封装的一个组件库，截止到目前，他可以搭配Vue2，Vue3，React，Augular等框架进行开发。

### 我觉得它的好处

他的官方文档对小白很友好，讲解的很细致，组件，组件的属性，提供的钩子，触发事件传的参数等等，都给出了详细的说明。

他提供了自己的icon图标，这样我们在使用的时候就会很方便，不用每个图标都要去阿里矢量库去找。

### 我觉得它的坏处

成也组件，败也组件，他的UI，权重特别高，导致我们将他的样式进行转换的时候就会很麻烦。

有时候组件会出bug，去年寒假我做一个Vue2+element-ui的项目时，他的`el-table`组件出了一个bug，在删减列后增加列，会导致index窜行或者窜位，导致有两排index同时出现的dom树中，后来向官方反应后，今年2月份的时候，我在看，bug已经消除了。

### 使用一下element-ui

在使用之前，我们要先去给项目添加element的依赖，有两种方式，可以去到我们的vue-ui面板，去插件与依赖中分别搜索element，去安装，也可以使用终端的npm工具进行下载依赖

```
npm i element-ui -S
```

完后去element抄一段代码，随便写在页面中（我写在了app.vue中）

```
<el-row>
  <el-button>默认按钮</el-button>
  <el-button type="primary">主要按钮</el-button>
  <el-button type="success">成功按钮</el-button>
  <el-button type="info">信息按钮</el-button>
  <el-button type="warning">警告按钮</el-button>
  <el-button type="danger">危险按钮</el-button>
</el-row>
```

我直接复制过来了。

粘贴完，直接`npm run serve`

是不是报错了？当然，因为我们还没有引入element到我们的项目中，我们只是安装了他的依赖而已。

在main.js中

```
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'

Vue.use(ElementUI)
```

上面两句是引入，下面的那句是使用。缺一不可。

假如在运行中遇到了这个错误，不要慌，这是你缺少了core.js的生产环境

![core-js](https://wangez.site/img/shangke/core-js.png)

在终端运行这个之后，继续运行你的vue项目，就不会报错了。

```
npm install --save core-js
```

好啦，让我们看看element的样子吧！

我的app.js：

```
<template>
  <div id="app">
      <el-row>
        <el-button>默认按钮</el-button>
        <el-button type="primary">主要按钮</el-button>
        <el-button type="success">成功按钮</el-button>
        <el-button type="info">信息按钮</el-button>
        <el-button type="warning">警告按钮</el-button>
        <el-button type="danger">危险按钮</el-button>
      </el-row>
  </div>
</template>

```

为了方便看效果，我把其他的都删了。

![element](https://wangez.site/img/shangke/element.png)

这样，我们的element就是引用成功的。

## 最后

组件库提高了前端人员的开发效率，这个是肯定的，但是这并不是说前端开发就不需要掌握CSS的知识了，组件不是万能的，在项目中，肯定要依据项目状态对组件有或多或少的修改，所以还是建议想学前端的同学要学好CSS，打好基础很重要（以后封装一个自己的组件库，还不是想怎么改就怎么改）。

