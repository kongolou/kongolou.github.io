---
title: Butterfly 主题初级配置
date: 2023-03-11 16:59:17
updated:
tags:
- butterfly
- theme
categories:
- 知识分享
keywords:
- hexo
- butterfly
- 主题配置
description: 本文介绍 Hexo | Butterfly 主题的初级配置，包括主题的安装与界面的配置等。
top_img:
comments:
cover: https://cdn.cdnjson.com/tvax3.sinaimg.cn//large/0072Vf1pgy1foxkcfw1pkj31hc0u017a.jpg
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
---

# 主题安装

## 主题下载

在博客根目录下运行 Bash ，从 Github 仓库克隆主题到本地。

```bash ../
$ git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

拷贝主题配置文件到根目录下，主题目录下的配置文件留作备份。

```bash ../
$ cp /themes/butterfly/_config.yml _config.butterfly.yml
```

## 应用主题并安装插件

修改全局配置主题为 butterfly 。

```yaml ../_config.yml
theme: butterfly
```

安装 pug 和 stylus 的渲染器以及 wordcount 插件。

```bash ../
$ npm install hexo-renderer-pug hexo-renderer-stylus hexo-wordcount --save
```

现在，主题已经应用到了你的网站。你可以通过以下指令在本地查看。

```bash ../
$ hexo cl && hexo g && hexo s
```

## 参考链接

[Butterfly 安装文档（一） 快速开始](https://butterfly.js.org/posts/21cfbf15/)

# 界面配置

## 修改 Front-matter

修改博客根目录下模板文件夹中两个文件的内容如下。

```markdown ../scaffolds/post.md
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
---
```

```markdown ../scaffolds/page.md
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
---
```

## 从模板创建

依次执行下面的指令，创建两篇博客和四个界面。

```bash ../
$ hexo new "hello-butterfly"
$ hexo new post "第一篇博客"
$ hexo new page tags
$ hexo new page categories
$ hexo new page link
$ hexo new page about
```

注意，默认从 post 模板创建，用于测试的博客文件可以自行删除。

将 tags 、 categories 以及 link 目录下 index.md 文件中 Front-matter 的 type 属性修改为对应的目录名称，例如：

```markdown
---
title: 标签
date: 2018-06-07 22:17:49
updated:
type: "tags"
comments:
description:
keywords:
top_img:
mathjax:
katex:
aside:
aplayer:
highlight_shrink:
orderby: random
order: 1
---
```

注意使用双引号，其他内容可以根据需求修改。

## 添加友情链接

新建数据文件夹与数据文件。

```bash ../
$ mk source/_data/link.yml
```

修改该文件，如添加 Hexo 官网链接。

```yaml source/_data/link.yml
- class_name: 官方网站
  class_desc: 让世界变得简单
  link_list:
    - name: Hexo
      link: https://hexo.io/zh-cn/
      avatar: https://d33wubrfki0l68.cloudfront.net/6657ba50e702d84afb32fe846bed54fba1a77add/827ae/logo.svg
      descr: 快速、简单且强大的网络框架
# 自定义
- class_name:
  class_desc:
  link_list:
    - name:
      link:
      avatar:
      descr:

```

## 参考链接

[Hexo 文档 | 写作](https://hexo.io/zh-cn/docs/writing)

[Butterfly 安装文档（二） 主题页面](https://butterfly.js.org/posts/dc584b87/)
