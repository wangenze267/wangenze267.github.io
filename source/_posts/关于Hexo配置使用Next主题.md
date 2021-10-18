---
title: 关于Hexo配置使用Next主题
author: Ned
tags:
  - Hexo
abbrlink: 2781919463
---

### 前言

之前写了个如何搭建Hexo的博客，后来想了想，既然写了就写到底吧，我自己用的是next这款主题，所以就说一下有关next的配置问题。并且，使用这个主题的过程中，我真的踩了不少的水坑！

### 确认一下版本号

`Hexo5.4.0`

`Next8.2.0`

版本不同可能会有些差异

<!-- more -->

### 安装next主题

去next团队的主页去下载一个zip压缩包

![image-20210216103416503](https://wangez.site/img/next.png)

完后将他放在`themes`目录下并且解压，重命名文件夹为`next`

`themes/`下是Hexo用来放主题的

### 修改站点配置文件

站点配置文件：`_config.yml`

参考Hexo文档中的说明，我们需要给博客设定主标题以及副标题、作者名称、语言、时区等，你可以参考以下我的设置。

```
# Site
title: Wangez-Blog  #网站标题
subtitle: 'Ned的个人博客'  #网站副标题
description: 'Ned的个人博客'  #网站描述
keywords:  #网站关键词
author: Ned  #您的名字
language: zh-CN  #语言
timezone: 'Asia/Shanghai'  #时区
```

#### 启用Next主题

在站点配置文件中，找到 theme 属性，然后修改为 next 即可

```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```

### 修改主题配置文件

主题配置文件：`themes/next/_config.yml`

#### 设定主题风格

Next提供了4种风格样式供我们选择。我个人偏好最经典且极简的Muse风格。你可以4种都体验以下。选择哪个风格，就删除掉对应风格名所在行前面的# ，可以参考我的配置：

```
# Schemes
# scheme: Muse
# scheme: Mist
scheme: Pisces
# scheme: Gemini
```

#### 博客主标题上方的logo

这个我是没有设置的，有需要的可以自己设置一下，代码如下：

```
# Custom Logo (Warning: Do not support scheme Mist)主页logo
custom_logo: /uploads/logo.png
```

顺便一提，后面我们还会遇到 引用图片地址 的类似设置。`/uploads/logo.png `对应的本地blog文件夹中位置是 `\blog\themes\next\source\uploads\logo.png` 其中 `uploads` 文件夹，和`logo.png`文件，都需要自己创建。

#### 版权信息说明

网上的太繁琐了，其实不用改太多。

```
# Creative Commons 4.0 International License.
# See: https://creativecommons.org/share-your-work/licensing-types-examples
# Available values of license: by | by-nc | by-nc-nd | by-nc-sa | by-nd | by-sa | zero
# You can set a language value if you prefer a translated version of CC license, e.g. deed.zh
# CC licenses are available in 39 languages, you can find the specific and correct abbreviation you need on https://creativecommons.org
creative_commons:
  license: by-nc-sa
  sidebar: true
  post: true
  language:
```

版权声明文本是可以修改的，后面会介绍到。

#### 设置导航栏菜单

我觉得设置多了太麻烦，于是就保留了`主页` `标签` `关于` `归档`四个

```
menu:
  home: / || fa fa-home
  about: /about/ || fa fa-user
  tags: /tags/ || fa fa-tags
  #categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  #schedule: /schedule/ || fa fa-calendar
  #sitemap: /sitemap.xml || fa fa-sitemap
  #commonweal: /404/ || fa fa-heartbeat

# Enable / Disable menu icons / item badges.
menu_settings:
  icons: true
  badges: false
```

启用哪个功能，就删除对应行前面的 # 

#### 侧边栏设置

我觉得左边挺好的，就没有改

```
sidebar:
  # Sidebar Position.
  position: left
  # position: right
```

设置侧边栏的头像，一般为作者的头像，和博客的logo有区别。

可以是方形或圆形，还可以选择鼠标停留时，魔性的转动（我没设置）。

```
# Sidebar Avatar
avatar:
  # Replace the default image and set the url here.
  url: /uploads/avatar.jpg #头像图片地址 
  # If true, the avatar will be dispalyed in circle.圆形选true 方形选false
  rounded: true
  # If true, the avatar will be rotated with the cursor.魔性转动，打开选true
  rotated: false
```

侧边栏外链，可以指向某篇文章，或某个网址。这里可以用来展示其他发布渠道。

```
social:
  #GitHub: https://github.com/yourname || fab fa-github
  #E-Mail: mailto:yourname@gmail.com || fa fa-envelope
  #Weibo: https://weibo.com/yourname || fab fa-weibo
  #Google: https://plus.google.com/yourname || fab fa-google
  #Twitter: https://twitter.com/yourname || fab fa-twitter
  #FB Page: https://www.facebook.com/yourname || fab fa-facebook
  #StackOverflow: https://stackoverflow.com/yourname || fab fa-stack-overflow
  #YouTube: https://youtube.com/yourname || fab fa-youtube
  #Instagram: https://instagram.com/yourname || fab fa-instagram
  #Skype: skype:yourname?call|chat || fab fa-skype

social_icons:
  enable: true  #用来控制是否显示图标的
  icons_only: false
  transition: false
```

和外链样式不同的是友链。默认显示为`友情链接`我将其修改为`我的朋友`。

```
# Blog rolls
links_settings:
  icon: fa fa-link
  title: 我的朋友
  # Available values: block | inline
  layout: block
#友情链接
links:
  Ned: https://wangez.site
```

#### 返回顶部按钮显示阅读进度

觉得没什么用，不过我还是用了（打脸ing）

```
back2top:
  enable: true
  # Back to top in sidebar.
  sidebar: false
  # Scroll percent label in b2t button.
  scrollpercent: true
```

#### 关闭动画效果

我觉得博客，渲染的太华丽反而不好，于是就将其关了，如果想打开可以将enable的值设置为`true`。

```
motion:
  enable: false
  async: false
```

到此，配置差不多就结束了，如果你还想优化你的博客，可以继续看下去。

### 优化你的博客

#### 优化文章链接

优化文章链接，为了避免链接中出现中文导致太长或者死链的情况出现，可以使用一个插件来避免这种情况

插件：`hexo-abbrlink`

首先在博客根目录运行Git Bash，输入以下指令安装`hexo-abbrlink`：

```
npm install hexo-abbrlink --save
```

打开站点配置文件`_config.yml`，修改permalink为：

```
permalink: posts/:abbrlink.html/
```

记得将原有的permalink注释掉或者删掉

在站点配置文件`_config.yml`中添加以下代码：

```
#abbrlink配置
abbrlink:
  alg: crc32 #support crc16(default) and crc32
  rep: dec   #support dec(default) and hex
```

到这，关于优化文章链接的操作我们就做完啦！

#### 自定义文本内容

文件路径：`themes/next/languages/zh-CN.yml`

需要打开这个文件进行修改。

选择哪个语言文档取决于你的站点配置文件上的语言写的是什么！

我将所有的`日志`换成了`文章`理由是我觉得日志怪怪的，还是文章比较正常。

Ctrl+H 进行批量替换。（不会真有人一个一个改吧，不会吧不会吧）

#### 添加访客统计、访问次数统计、文章阅读次数统计

打开主题配置文件

搜索找到`busuanzi_count`，把`enable`设置为`true`

```
# Show Views / Visitors of the website / page with busuanzi.#展示访问数
# Get more information on http://ibruce.info/2015/04/04/busuanzi
busuanzi_count:
  enable: true
  total_visitors: true   #统计访客数
  total_visitors_icon: user
  total_views: true    #统计访问数
  total_views_icon: eye
  post_views: true   #统计文章阅读数
  post_views_icon: eye
```

同样是在主题配置文件下，搜索`footer`，在它底下添加`counter`，设值为`true`

```
  #统计
  counter: true
```

**这个我一开始配置了，但是不生效，最后将它删了才生效的，如果你配置完有问题也可以将其删了试一试**

来到`themes\next\layout_partials`，找到`footer.swig`文件，打开编辑，在底下添加代码

```
{% if theme.footer.counter %}
    <script async src="//dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>
{% endif %}
```

这样，就设置完成了，站点访客数、访问次数显示在网址底部，文章阅读次数在文章开头。

