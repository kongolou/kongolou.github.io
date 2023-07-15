---
title: Butterfly 主题配置指北
date: 2023-07-14 16:16:38
updated:
tags:
  - Butterfly
  - Blog
categories:
  - 沉淀
  - 指北
keywords:
  - Butterfly
  - 主题
description:
top_img:
comments:
cover:
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
abcjs:
---
# 写在前面

主题配置是很个性化的东西，你可以在 Hexo 官网找到很多好看的主题。  
如果你和我一样选择使用 Butterfly 主题，那么[官方配置文档](https://butterfly.js.org/)无疑是最佳参考。

# 网站配置

先在 Hexo 的配置文件 `blog/_config.yml` 中完善个人 Site 及 URL 信息。  
```yaml
title: "KGL.BLOG"
subtitle: "Kongolou's Blog"
description: '你好！这里是 kongolou 的个人博客 ~'
keywords:
  - kongolou
  - kgl.blog
author: kongolou
language:
  - zh-CN
  - zh-TW
  - en
timezone: ''

url: http://kongolou.github.io
permalink: /post/:title.html
```

# 安装主题 [？](https://butterfly.js.org/posts/21cfbf15/)

安装主题可以采用多种方式，这里采用 npm 安装。  
```bash
$ npm install hexo-theme-butterfly
$ cp node_modules/hexo-theme-butterfly/_config.yml _config.butterfly.yml
```
注：在 Butterfly 的新版本中，应原始包含 `hexo-renderer-pug` 和 `hexo-renderer-stylus` 两个组件。

# 新建模板 [？](https://butterfly.js.org/posts/dc584b87/)

对于 Front-matter 中的详细参数，请戳“新建模板”标题旁的小问号加以了解。

## 页面模板

修改 `blog/scaffolds/page.md` 文件。  
```yaml
---
title: {{ title }}
date: {{ date }}
updated:
type:
comments:
description:
keywords:
top_img:
mathjax:
katex:
aside:
aplayer:
highlight_shrink:
random:
---
```

## 写作模板

修改 `blog/scaffolds/post.md` 文件。  
```yaml
---
title: {{ title }}
date: {{ date }}
updated:
tags:
categories:
keywords:
description:
top_img:
comments:
cover:
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
abcjs:
---
```

# 页面导航

## 新建界面 [？](https://butterfly.js.org/posts/dc584b87/)

这里创建几个简单的页面。
```bash
$ hexo new page archives
$ hexo new page tags
$ hexo new page categories
$ hexo new page about
```
然后分别修改这些页面在 `blog/source/` 目录中对应文件夹下的 `index.md` 文件。
```yaml
---
title: 归档
date: 2023-03-10 00:00:00
comments: false
---
```
```yaml
---
title: 标签
date: 2023-03-10 00:00:00
type: "tags"
comments: false
---
```
```yaml
---
title: 分类
date: 2023-03-10 00:00:00
type: "categories"
comments: false
---
```
```yaml
---
title: 关于
date: 2023-03-10 00:00:00
comments: false
---
你抓到我了。
```

你也可以添加其他页面，如友链页面、图库页面等。详情请戳小问号。

## 修改配置 [？](https://butterfly.js.org/posts/4aa8abbe/)

修改主题配置文件 `blog/_config.butterfly.yml` 相关内容如下。  
```yaml
nav:
  logo:
  display_title: true
  fixed: false

menu:
  主页: / || fas fa-home
  目录 || fas fa-compass || hide:
    归档: /archives/ || fas fa-archive
    标签: /tags/ || fas fa-tags
    分类: /categories/ || fas fa-folder-open
  关于: /about/ || fas fa-heart
```

相信你已经认识小问号了。

# 图片设置 [？](https://butterfly.js.org/posts/4aa8abbe/)

首先新建 `source/image/` 并加入你喜欢的图片，建议统一存储为 PNG 格式。  
然后将要作为展示的图像命名为 `favicon.png`、`avatar.png`、`default_top_img.png`、`default_cover.png` 等形式，这样以后更换对应图像只需进行重命名操作即可。

## 网站图标

```yaml
favicon: /image/favicon.png
```

## 个人头像

开启特效后，头像会一直转圈，这里选择关掉特效。  
```yaml
avatar:
  img: /image/avatar.png
  effect: false
```

## 网页背景

可以对每个页面分别设置背景，这里选择将全部页面的背景设置成同一个默认背景，并覆盖页脚。  
```yaml
disable_top_img: false
default_top_img: /image/default_top_img.png
footer_bg: true
```

## 文章封面

这里设置不显示文章封面。  
```yaml
cover:
  index_enable: false
  aside_enable: false
  archives_enable: false
  position: left
  default_cover:
    - /image/default_cover.png
```

## 图片加载

为防止出现“断层”现象，可以设置网页预加载，1 为全屏加载动画，2 为进度条
```yaml
preloader:
  enable: true
  source: 1
  pace_css_url:
```

# 社交设置 [？](https://butterfly.js.org/posts/4aa8abbe/)

```yaml
social:
  fab fa-bilibili: https://space.bilibili.com/453346061 || Bilibili
  fab fa-github: https://github.com/kongolou || Github
  fas fa-envelope: mailto:kongolou@163.com || Email
```

# 侧栏设置

这里选择关掉按钮、公告以及其他不必要的内容。  
```yaml
aside:
  enable: true
  card_author:
    enable: true
    button:
      enable: false
  card_announcement:
    enable: false
  card_categories:
    enable: false
  card_tags:
    enable: false
  card_archives:
    enable: false
```

# 页脚设置

```yaml
footer:
  owner:
    enable: true
    since: 2023
  custom_text:
  copyright: true
```

# 特效设置

## 主页特效

```yaml
subtitle:
  enable: true
  effect: true
  typed_option:
  source: 1
  sub:
```

## 背景特效

这里选择不设置背景特效。

## 点击特效

我这里开一个烟花特效。  
```yaml
fireworks:
  enable: true
  zIndex: 9999 # -1 or 9999
  mobile: false
```

# 写作设置

## 文章摘要

这里设置不显示摘要  
```yaml
index_post_content:
  method: false
```

# 文章设置

## 页面美化

```yaml
beautify:
  enable: true
  field: post
  title-prefix-icon: '\f0c1'
  title-prefix-icon-color: '#F47466'
```

## 相关文章

这里选择关掉显示相关文章。  
```yaml
related_post:
  enable: false
  limit: 6
  date_type: created

post_pagination: false
```

# 插件设置

## 字数统计

```bash
$ npm install hexo-wordcount --save
```
修改 `blog/_config.butterfly.yml` 文件。  
```yaml
wordcount:
  enable: true
  post_wordcount: true
  min2read: true
  total_wordcount: true
```

## 音乐播放 [？](https://butterfly.js.org/posts/507c070f/)

```bash
$ npm install hexo-tag-aplayer --save
```
修改 `blog/_config.yml` 文件。  
```yaml
# Extensions
## Plugins: https://hexo.io/plugins/
aplayer:
  meting: true
  asset_inject: false
```
修改 `blog/_config.butterfly.yml` 文件。  
```bash
aplayerInject:
  enable: true
  per_page: true
inject:
  head:
  bottom:
    - <div class="aplayer no-destroy" data-id="1671397006" data-server="tencent" data-type="playlist" data-fixed="true" data-mini="true"> </div>
```

## 数学公式 [？](https://butterfly.js.org/posts/ceeb73f/)

```bash
$ npm uninstall hexo-renderer-marked --save
$ npm uninstall hexo-renderer-kramed --save
$ npm install hexo-renderer-markdown-it --save
$ npm install katex @renbaoshuo/markdown-it-katex
```
修改 `blog/_config.yml` 文件。  
```yaml
# Extensions
## Plugins: https://hexo.io/plugins/
markdown:
  plugins:
    - '@renbaoshuo/markdown-it-katex'
```
注：按照这种方法生成的公式似乎存在 bug，目前不建议使用数学公式。

# 参考链接

- [Hexo 文档 | Front-matter](https://hexo.io/zh-cn/docs/front-matter)
- [Butterfly 官网](https://butterfly.js.org/)
