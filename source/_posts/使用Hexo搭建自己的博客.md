---
title: 使用Hexo搭建自己的博客
author: Ned
tags:
  - Hexo
  - 踩坑日记
abbrlink: 735412972
---

### 前言

之前一直在用typecho来做自己的博客，因为他操作比较简单，但是前几日修改一些配置的时候，看着满屏的php代码实在有些头疼，在朋友的推荐下，我成功的入坑了hexo，下面分享一些自己搭建博客的过程，尽量让读者避开一些坑。

### Hexo简介

Hexo是一款基于Node.js的静态博客框架，依赖少易于安装使用，可以方便的生成静态网页托管在GitHub和Coding上，是搭建博客的首选框架。大家可以进入[hexo官网](https://hexo.io/zh-cn/)进行详细查看，因为Hexo的创建者是台湾人，对中文的支持很友好，可以选择中文进行查看。

<!-- more -->

### 开始搭建

在安装hexo前，电脑上具备有git与node两个环境才行

#### 安装git

Git是目前世界上最先进的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。也就是用来管理你的hexo博客文章，上传到GitHub的工具。Git非常强大，我觉得建议每个人都去了解一下。廖雪峰老师的Git教程写的非常好，大家可以了解一下。[Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

windows：到git官网上下载,[Download git](https://gitforwindows.org/),下载后会有一个Git Bash的命令行工具，以后就用这个工具来使用git。

linux：对linux来说实在是太简单了，因为最早的git就是在linux上编写的，只需要一行代码

```
sudo apt-get install git
```

#### 安装node

Hexo是基于nodeJS编写的，所以需要安装一下nodeJs和里面的npm工具。

windows：[nodejs](https://nodejs.org/en/download/)  

linux:

```
sudo apt-get install nodejs
sudo apt-get install npm
```

安装完后，打开命令行

```
node -v
npm -v
```

检查一下有没有安装成功

顺便说一下，windows在git安装完后，就可以直接使用git bash来敲命令行了。

#### 安装hexo

现在我们就可以安装hexo了

前面git和nodejs安装好后，就可以安装hexo了，你可以先创建一个文件夹blog，然后`cd`到这个文件夹下（或者在这个文件夹下直接右键git bash打开）。

输入命令

```
npm install -g hexo-cli
```

依旧用`hexo -v`查看一下版本

至此就全部安装完了。

接下来初始化一个blog吧！

```
hexo init myblog(myblog是你创建的文件夹的名字，可以随意命名)
```

然后

```
cd myblog //进入这个myblog文件夹
npm install
```

新建完成后，指定文件夹目录下有：

- node_modules: 依赖包
- public：存放生成的页面
- scaffolds：生成文章的一些模板
- source：用来存放你的文章
- themes：主题
- _config.yml: 博客的配置文件

```
hexo g
hexo server
```

打开hexo的服务，在浏览器输入`localhost:4000`就可以看到你生成的博客了。

使用ctrl+c可以把服务关掉。

注：可以使用 `hexo command` 来查看hexo全部命令

### 准备好GitHub

首先，你先要有一个GitHub账户，去注册一个吧。

注册完登录后，在GitHub.com中看到一个New repository，新建仓库

创建一个和你用户名相同的仓库，后面加.github.io，只有这样，将来要部署到GitHub page的时候，才会被识别，xxx.github.io，其中xxx就是你注册GitHub的用户名。

点击create repository。

回到你的git bash中

```
git config --global user.name "yourname"
git config --global user.email "youremail"
```

这里的yourname输入你的GitHub用户名，youremail输入你GitHub的邮箱。这样GitHub才能知道你是不是对应它的账户。

可以用以下两条，检查一下你有没有输对:

```
git config user.name
git config user.email
```

### 将hexo部署到GitHub

这一步，我们就可以将hexo和GitHub关联起来，也就是将hexo生成的文章部署到GitHub上，打开站点配置文件 `_config.yml`，翻到最后，修改为
YourgithubName就是你的GitHub账户

```
deploy:
  type: git
  repo: https://github.com/YourgithubName/YourgithubName.github.io.git
  branch: master
```

这个时候需要先安装deploy-git ，也就是部署的命令,这样你才能用命令部署到GitHub。

```
npm install hexo-deployer-git --save
```

然后：

```
hexo clean
hexo generate
hexo deploy
```

其中 `hexo clean`清除了你之前生成的东西，也可以不加。
`hexo generate` 顾名思义，生成静态文章，可以用 `hexo g`缩写
`hexo deploy` 部署文章，可以用`hexo d`缩写

注意deploy时可能要你输入username和password。

过一会儿就可以在`http://yourname.github.io` 这个网站看到你的博客了！

### 关联个人域名

如果你想自己的域名与博客也关联起来的话，可以看这里

在source里创建一个叫CNAME的文件（注意没有任何后缀，名字就叫CNAME）

在CNAME里面写上你的域名，例如：

> blog.wangez.site

之后打开git bash

```
hexo clean
hexo g
hexo d
```

在GitHub上你创建的仓库里就会多一个叫做CNAME的文件了

过不了多久，再打开你的浏览器，输入你自己的域名，就可以看到搭建的网站啦！