---
title: 关于Hexo使用Next主题的一些优化
author: Ned
tags:
  - Hexo
abbrlink: 1559886533
---

### 前言

肯定还会跟我一样，使用主题之后还不满意的小伙伴，不光想要好，还想要更好！

那就来看看如何优化你的博客，让他变得更富有细节吧 ~

#### 优化文章链接

优化文章链接，为了避免链接中出现中文导致太长或者死链的情况出现，可以使用一个插件来避免这种情况

插件：`hexo-abbrlink`

<!-- more -->

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

