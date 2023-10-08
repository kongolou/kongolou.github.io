---
title: Vim-配置
date: 2023-10-06 10:56:38
updated:
tags:
  - Vim
categories:
  - 沉淀
  - 手记
keywords:
  - Vim
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
以下是我个人使用的一些 gVim 配置。

在 _vimrc 文件中追加如下内容。

```vim
" ... 在结尾追加
" 设置缩进
set shiftwidth=4
set expandtab
set softtabstop=-1
" 设置字体
" 查看字体 :set guifont
set guifont=Consolas:h12:cANSI:qDRAFT
" 设置主题
" 查看主题 :colorscheme
colorscheme desert
```

# 参考链接
- [linux - vim技巧：详解tabstop、softtabstop、expandtab三个选项的区别 - 南木阁 - SegmentFault 思否](https://segmentfault.com/a/1190000021133524)
