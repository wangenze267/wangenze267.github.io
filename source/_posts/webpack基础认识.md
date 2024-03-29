---
title: webpack基础认识
author: Ned
tag: webpack
abbrlink: 3206731080
---

### 前言

webpack在框架中多半被脚手架所集成，例如vue中的vue-cli，但是在面试中，还是会被问到，所以我们还是要了解一些必要知识，能让我们对此类技术有一个更深的认识。

不得不说，在学习过程中，我了解到了许多我之前仅仅知道那是配置文件的东西，但是经过学习了解之后，已经可以知道哪些配置是为了什么目的的了。

> 古人云：学吧，学会了都是你的~
>
> 古人又云：学吧，学不死就往死里学！

*注：本文为摘录笔记，供学习使用。*

<!--more-->

### 什么是webpack

可能大家了解的，百度搜到的，webpack是一个**模块打包器**。它将根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。

还有另一种说法：webpack是**前端项目工程化的具体解决方案**

主要功能：它提供了友好的**前端模块化**开发支持，以及**代码压缩混淆、处理浏览器端JavaScript的兼容性、性能优化**等强大的功能。

> - 代码压缩混淆是为了缩减文件体积，让网页打开更迅速
> - 程序员可以放心大胆写js高级语法，webpack在具体浏览器运行的时候会有转换过程，将高级的转换成低级，没有兼容问题的代码，去浏览器里跑。
> - css3动画，浏览器不兼容是不会显示的，不会报错，效果出不来
> - js，如果低版本浏览器不兼容的话，浏览器会报错
> - webpack目前有一些现有的成熟的兼容方案

好处：让 程序员把**工作重心**放到具体功能的实现上，提高了前端**开发效率**和项目的**可维护性**。

### webpack的基本使用

#### 构建初始项目目录

因为许多我觉得大家都会，所以在此简略一些。

①新建项目空白目录，运行`npm init -y`命令，初始化包管理配置文件`package.json`

②新建`src`源代码目录

③新建src--->`index.html`首页和src--->`index.js`脚本文件

> 下面就是初始化和引入依赖了，我随便的引入一个jq，完后瞎写了点东西，主要还是学习webpack

④初始化首页的基本结构

⑤运行`npm install jquery -S`命令，安装jQuery

> 之前是不是都是去网上下一个类似`jquery.min.js`的文件手动拉到文件夹？

#### 在项目中安装webpack

在终端运行如下命令：

```js
npm install webpack@5.42.1 webpack-cli@4.7.2 -D
```

>解释一下npm命令中-S -D的区别
>
>开发中跟上线后部署都需要的包，要-S存到dependencies节点下
>
>只在开发中使用的包，上线后不需要的包，要-D存到devDependencies节点下
>
>注：-S 是 --save的简写 -D 是 --save-dev的简写

 #### 在项目中配置webpack

只安装不配置当然不会生效，所以我们还要来配置一下。

① 在项目根目录中，创建名字为`webpack.config.js`的webpack配置文件，并初始化如下的基本配置。

```js
module.exports = {
	mode: 'development' // mode用来指定构建模式，可选值：development、production
}
```

module.exports  这个是不是很眼熟（Node.js的导出语法，如果对node有了解的话会知道），它的作用是向外导出一个配置对象，它是webpack的配置文件，当然，这个配置就是给webpack用的咯！

mode的两个可选值，代表着开发的不同阶段：分别是开发模式（development）跟生产模式（production）。

> 生产模式也就是要上线了。

② 在package.json的scripts节点下，新增`dev脚本`如下：

```js
"scripts":{
	"dev":"webpack" // script 节点下的脚本，可通过npm run执行，例如npm run dev
}
```

`"dev":"webpack"`,冒号前面是这个脚本的名字，冒号后面是个命令，后面的命令必须是字符串，前面脚本的名字可以任意去写，不一定叫dev，合法就行。

③ 在终端中运行`npm run dev命令`，启动webpack进行项目的打包构建。

运行之后，会多一个`dist`目录，打开后里面有一个main.js代码。

如果你之前`index.js`的代码引入到`index.html`中，出现了兼容性问题，那么不妨引入`main.js`试试，你会发现没有任何问题，这就是**webpack**的在起作用了，它帮着对我们的项目进行了兼容性处理。

#### mode的可选值

**mode节点**的可选值在上面已经说过了，有两个，分别是

- development
  - 开发环境
  - 不会对打包生成的文件进行代码压缩和性能优化
  - 打包速度快，适合在开发阶段使用
- production
  - 生产环境
  - 会对打包生成的文件进行代码压缩和性能优化
  - 打包速度很慢，今是何在项目发布的阶段使用

**结论：**开发的时候我们一定要用development，因为我们追求的是速度而不是体积，反过来，发布上线的时候我们一定要用production，因为上线追求的是体积和性能，而不是打包的速度。

#### webpack.config.js文件的作用

webpack.config.js是webpack的配置文件。webpack在真正开始打包构建之前，会先读取这个配置文件，从而基于给定的配置，对项目进行打包。

注意：由于webpack是基于node.js开发出来的打包工具，因此在它的配置文件中，支持使用node.js相关的语法和模块进行webpack的个性化配置。

#### webpack中的默认约定

在webpack4.x和5.x的版本中，有如下的默认约定：

- 默认的打包入口文件为`src->index.js`
- 默认的输出文件路径为`dist->main.js`

注意：可以在`webpack.config.js`中修改打包的默认约定

#### 自定义打包的入口与出口

在`webpack.config.js`配置文件中，通过**entry**节点指定打包的入口。通过**output**节点指定打包的出口。

示例代码如下：

```js
const path = require('path') // 导入node.js中专门操作路径的模块

module.exports = {
    entry: path.join(__dirname,'./src/index.js'), // 打包入口文件路径
    output: {
        path: path.join(__dirname, './dist'), // 输出文件的存取路径
        filename: 'main .js' // 输出文件名称 
    }
}
```

> 注意： __dirname 是两个下划线！！代表这个文件的存放路径。

### webpack中的插件

> 方便程序员的开发

通过安装和配置第三方的插件，可以拓展webpack的能力，从而让webpack用起来方便。最常用的webpack插件有如下两个：

- webpack-dev-server
  - 类似于node.js阶段用到的nodemon工具
  - 每当修改了源代码，webpack会自动进行项目的打包和构建
- html-webpack-plugin
  - webpack中的HTML插件（类似于一个模板引擎插件）
  - 可以通过此插件自定制index.html页面内容

#### 安装webpack-dev-server

运行如下命令：

`npm install webpack-dev-server@3.11.2 -D`

> -D，上面有讲过，他只是在开发阶段使用的工具，并不是上线部署的时候需要的。

 #### 配置webpack-dev-server

- 修改`package.json-->scripts`中的`dev`命令如下

  ```js
  "scripts":{
  	"dev": "webpack serve", // script 节点下的脚本，可以通过npm run 执行
  }
  ```

> serve是一个参数，代表我们要通过插件实现自动打包功能，只要修改代码保存，他就会自动打包生成

- 再次运行`npm run dev`命令，重新进行项目的打包
- 在浏览器中访问`http://localhost:8080`地址，查看自动打包效果

注意：`webpack-dev-server`会启动一个**实时打包的http服务器**

