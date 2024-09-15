---
title: Johnny 杂记
date: 2024-09-15 19:04:23
updated:
tags:
  - Johnny
  - John the Ripper
categories:
  - 沉淀
  - 手记
keywords:
  - Johnny
  - John the Ripper
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
# 安装 John the Ripper
Johnny 是 John the Ripper 的一个前端界面。安装 Johnny 首先需要安装 John the Ripper。

访问官方网站 [John the Ripper password cracker](https://www.openwall.com/john/) 获取最新版本的 John the Ripper。

# 安装 Johnny

访问官方网站 [Johnny - GUI for John the Ripper](https://openwall.info/wiki/john/johnny) 获取最新版本的 Johnny。

打开 Johnny，在 Settings 中设置 John the Ripper 的路径。如 `D:/Toolbox/john-1.9.0-jumbo-1-win64/run/john.exe`。

# 暴力破解

破解文件密码如 zip，在 Johnny 中：`Open password file` -> `Open another file format (*2john)` -> `Choose file format` -> `zip` -> 选择目标文件进行转化和破解。
