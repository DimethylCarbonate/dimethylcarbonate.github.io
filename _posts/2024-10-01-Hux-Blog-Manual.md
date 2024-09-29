---
layout:             post
title:              Hux Blog用户手册
subtitle:           如何快速构建个人博客
date:               2024-10-01
author:             Dimethyl
original:           true
header-style:       text
catalog:            true
tags:
    - Blog
---

## 前言

高二的时候心血来潮突然有了构建自己博客的想法，于是逛了逛[GitHub](https://github.com/)，发现了[Hux Blog](https://github.com/huxpro),于是便有眼前的GitHub Pages。

时光悄然而逝。又一次心血来潮。不妨把这篇指南翻译一下，打发晚上的闲暇时光。

原文参见本博客的[Github 仓库](https://github.com/DimethylCarbonate/dimethylcarbonate.github.io)或Hux Blog。

本文档原有目录部分。考虑到博客有侧边栏目录功能，故将其删去。

三脚猫英语水平，也使用了AI进行辅助，有错请反馈。

## Hux Blog 用户手册

### 准备开始

1. 使用[Jekyll](https://jekyllrb.com/)需要用到[Ruby](https://www.ruby-lang.org/en/)和[Bundler](https://bundler.io/)。请按照[Using Jekyll with Bundler](https://jekyllrb.com/tutorials/using-jekyll-with-bundler/)以满足环境需求。

2. 在`Gemfile`中安装依赖(dependencies)：

```sh
$ bundle install 
```

3. 启动网站(默认`localhost:4000`)：

```sh
$ bundle exec jekyll serve  # alternatively, npm start
```

### 开发

要修改主题，你需要[Grunt](https://gruntjs.com/)。在`Gruntfile.js`中有许多任务，包括压缩JavaScript、将`.less`编译为`.css`、添加横幅（banners）以保持Apache 2.0许可的完整性、监视更改等。

是的，它们是继承的，非常过时，没有模块化和转译等。

关键的Jekyll相关代码位于`_include/`和`_layouts/`。它们大多数是[Liquid](https://github.com/Shopify/liquid/wiki)模板。

这个主题使用Jekyll的默认代码语法高亮器[Rouge](http://rouge.jneen.net/)，与Pygments主题兼容，因此只需选择任何Pygments主题css（例如从[这里](http://jwarby.github.io/jekyll-pygments-themes/languages/javascript.html)），并替换`highlight.less`的内容。

### 配置

你可以通过修改`_config.yml`来轻松自定义博客：

```yml
# Site settings
title: Hux Blog             # 你网站标题
SEOTitle: Hux Blog          # 查阅文档了解更多细节
description: "Cool Blog"    # ...

# SNS settings      
github_username: huxpro     # 将该账户修改为你自己的
weibo_username: huxpro      # 页脚将自动更新

# Build settings
paginate: 10                # 每页文章数量
```

更多选项，请查看[Jekyll - Official Site](http://jekyllrb.com/)。大多数都非常直观，所以也可以放心直接查看代码。

### 文章

文章只是`_posts/`中的Markdown文件。
文章的元数据以YAML风格的_front-matter_列出。

例如，[Hello 2015](https://huangxuan.me/2015/01/29/hello-2015/)的front-matter如下：

```yml
---
layout:     post
title:      "Hello 2015"
subtitle:   " \"Hello World, Hello Blog\""
date:       2015-01-29 12:00:00
author:     "Hux"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Life
    - Meta
---
```

> 注意：`tags`部分也可以写成`tags: [Life, Meta]`。

在引入[Rake](https://github.com/ruby/rake)后，我们可以使用下面的命令简化文章创建：

```
rake post title="Hello 2015" subtitle="Hello World, Hello Blog"
```

此命令将自动在`_posts/`文件夹下生成一个类似上面的示例文章。

有许多*高级*配置：

1. *文本*样式的标题， 如[这个](https://huangxuan.me/2019/09/08/spacemacs-workflow/) 

```yml
header-style: text 
```

2. 开启$\LaTeX$支持：

```yml
mathjax: true
```

3. 给标题图片添加遮罩(mask)：

```yml
header-mask: 0.3
```

等等。

### 侧边栏

**侧边栏** 提供了可能的模块，以展示更多个人信息。

```yml
# Sidebar settings
sidebar: true   # 默认为true
sidebar-about-description: "your description here"
sidebar-avatar: /img/avatar-hux.jpg     # 使用独立URL
```

默认启用了 *[Featured Tags](#featured-tags)*、*[Mini About Me](#mini-about-me)* 和 *[Friends](#friends)* 模块，你也可以添加你自己的。侧边栏是自适应的，即在较小的屏幕（<= 992px，根据[Bootstarp Grid System](http://getbootstrap.com/css/#grid)）下会被推到底部。

### 迷你“关于”

如果设置了`sidebar-avatar`和`sidebar-about-description`变量，**迷你“关于”** 将会显示你的头像、描述和所有SNS按钮。在较小的屏幕中，当整个侧边栏被推到底部时，它会隐藏，因为页脚已经有SNS部分了。

### 特色标签

**特色标签**类似于[Medium](http://medium.com)等网站上的任何酷标签功能。从V1.4开始，即使侧边栏关闭，这个模块也可以使用，并且始终显示在底部。

```yml
# Featured Tags
featured-tags: true  
featured-condition-size: 1     # 如果标签大小超过此条件值，则该标签将被显示
```

唯一需要注意的是`featured-condition-size`，它表示一个标准，标签需要符合才能被显示。

### 朋友

朋友是任何博客的常见功能。如果你和你朋友的网站有双向超链接，它有助于SEO。即使侧边栏关闭，这个模块也可以发挥作用。朋友信息配置为`_config.yml`中的JSON字符串Friends。

```yml
# Friends
friends: [
    {
        title: "Foo Blog",
        href: "http://foo.github.io/"
    },
    {
        title: "Bar Blog",
        href: "http://bar.github.io"
    }
]
```

### Keynote布局

使用Reveal.js、Impress.js、Slides、Prezi等通过Open Web技术进行keynote和演示的趋势日益增加。我认为一个现代博客应该对这些基于嵌入HTML的演示有一流的支持，所以制作了**Keynote布局**。

可在**front-matter**中使用：

```yml
---
layout:     keynote
iframe:     "http://huangxuan.me/js-module-7day/"
---
```

iframe元素将自动调整大小以适应不同的形态因素和设备方向。因为大多数keynote框架阻止了浏览器的默认滚动行为。设置了底部填充以帮助并提示用户下面可能有更多的内容。

### 评论

> 寻求帮助：转移到基于Github的解决方案。

目前，[Disqus](http://disqus.com)作为第三方评论系统得到支持。
首先，你需要注册并拥有自己的账户。**重复一遍，不要使用我的！** （我设置了受信任的域）注册非常简单，你将获得完整的管理系统权限。请试试看！

其次，从V1.5开始，你可以通过在`_config.yml`中添加你的**短名称**来轻松完成你的评论配置：

```yml
duoshuo_username: _your_duoshuo_short_name_
# 或者
disqus_username: _your_disqus_short_name_
```

**对于旧版本用户**，最好拉取新版本，否则你必须自己替换`post.html`、`keynote.html`和`about.html`中的代码。

### 分析

从V1.5开始，Google Analytics和Baidu Tongji都可以通过简单的配置得到支持：

```yml
# Baidu Analytics
ba_track_id: 4cc1f2d8f3067386cc5cdb626a202900

# Google Analytics
ga_track_id: 'UA-49627206-1'            # 格式：UA-xxxxxx-xx
ga_domain: huangxuan.me
```

只需查看Google/Baidu提供的代码，复制粘贴到这里，其余的都已经为你完成了。（Google可能需要元标签`google-site-verification`）

### SEO标题

在V1.4之前，站点设置title不仅用于在首页和导航栏中显示，还用于生成HTML中的`<title>`。
你可能希望这两个不同。对我来说，我的网站标题是 **“Hux Blog”**，但我希望搜索引擎显示的标题是多语言的 **“黄玄的博客 | Hux Blog”**。

因此，引入了SEO标题来解决这个问题，你可以设置`SEOTitle`与`title`不同，它仅用于生成HTML的`<title>`标签和设置DuoShuo Sharing。

## 常见问题解答（FAQ）

#### cannot load such file -- jekyll-paginate

这个博客始于Jekyll 2时代，当时`jekyll-paginate`是标准插件。到了Jekyll 3，它成为了我们在`_config.yml`中包含的一个插件。确保你通过纯`gem`命令行界面或Bundler安装了它。

## 发布（Releases）

#### V1.8.2

- 合并了@JinsYin的#333和#336。
- 由于越来越多的人使用Bundler，添加了`Gemfile`。
- 待办事项：`multilingual` 功能可以通过配置和约定更加自动化。
- 在彻底重写更好的`project`页面之前，删除了整个`portfolio`页面。

#### V1.8.1

- 改进了多语言实现，可参见`about.html`或`_posts/2017-07-12-upgrading-eleme-to-pwa.markdown`了解用法示例。

#### V1.8

- 全新的[存档](https://huangxuan.me/archive/)页面！它结合了之前的存档和标签页面，并且保持向后兼容。 感谢[@kitian616/jekyll-TeXt-theme](https://github.com/kitian616/jekyll-TeXt-theme)提出了这个想法。
- 通过提取重复的Liquid模板到可重用的include中，改进了工程。这是@Kaijun在#74中提出的，但推迟了整整2.5年！由于长时间的分歧，我无法直接合并他的PR，但功劳属于@Kaijun。
- 改进了代码块。长久以来一直想要的行号现在开箱即用（感谢@SmilingParadise在新浪微博的帮助），默认主题也更新为Atom One Dark（请参阅常见问题解答（FAQ），了解如何更改为你最喜欢的主题）。
- @Voleking在#80中增加了MathJax支持。我选择使用SVG渲染器。有关写作和转义的细节，请参阅[Mathjax, kramdown and Octopress](https://www.lucypark.kr/blog/2013/02/25/mathjax-kramdown-and-octopress/)。
- @Android-KitKat在#253中增加了Open Graph协议支持。
- `header-img-credit`和`header-img-credit-href`。
- `nav-style: invert`和`header-style: text`。

#### V1.7

- 支持PWA / Service Worker。

#### v1.6

- 为了更好地支持HTTPS，将CDN更换为cdnjs。

#### V1.5.2

- 克隆或拉取后是否觉得删除我的博客文章很麻烦？试试**样板（Beta）**，它可以帮助你快速开始，并轻松合并更新。
- 在字体规则中添加了`-apple-system`，这将在iOS 9中默认显示美观的新字体**San Francisco**。
- 修复了关于代码换行的[问题#15](https://github.com/Huxpro/huxpro.github.io/issues/15)。

#### V1.5.1

- **[评论](#评论)** 正式支持[**Disqus**](http://disqus.com)，感谢@rpsh。

#### V1.5

- **[评论](#评论)** 和 **[分析](#分析)**现在可以配置了！我们还增加了**Google Analytics support**并放弃了腾讯分析。两个文件都已更新。

#### V1.4

- **[特色标签](#特色标签)** 现在独立于[侧边栏](#侧边栏)。两个文件都已更新。
- 引入了不同于网站标题的新 **[SEO标题](#seo标题)**。

#### V1.3.1

- 支持**PingFang (苹方)**，这是[OS X El Capitan](http://www.apple.com/cn/osx/whats-new/)中推出的新中文字体。

#### V1.3

- **导航菜单**的大幅改进 *（特别是在Android上）*：摒弃了老旧、卡顿、低性能的[Bootstrap collapse.js](http://getbootstrap.com/javascript/#collapse)，换成了我们自己编写的、[无jank](http://jankfree.org/)的navbar菜单，这是一个非常高性能的[Google Material Design](https://www.google.com/design/spec/material-design/introduction.html)实现。

#### V1.2

- 提供了全新的 **[keynote布局](#keynote布局)** ，方便你发布用这个博客创建的精美HTML演示文稿。

#### V1.1

- 我们现在支持一个干净且华丽的 **[侧边栏](#侧边栏)**，用于显示更多信息。
- **[朋友](#朋友)** 功能也作为博客的一个常见功能添加进来，帮助你进行SEO。

#### V1.0

- 全功能的**标签**支持。
- **移动优先**用户体验优化。
- 针对中文字体的**排版优化**。
- 针对中国的**网络优化**，放弃Google webfont，使用本地CDN。
- 使用[Github Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/)。
- 使用百度、腾讯/QQ分析。
- 使用[DuoShuo](http://duoshuo.com/)作为类似Disqus的第三方评论系统。

## Dimethyl 开发的新功能

Dimethyl在研究代码的时候也作出了一些改进。这里记录下一些重要的修改。

### d-1.1

1. 全模板汉化。
2. 优化了作者及版权信息展示。若该文章为原创文章，可在每篇`post`中的属性界面加入`original`属性；若是转载文章，可在属性界面中加入`source`属性。网页会显示对应的内容。
