---
title: Kodi 杂记
date: 2024-05-26 17:07:20
updated:
tags:
  - Kodi
categories:
  - 沉淀
  - 手记
keywords:
  - Kodi
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
# 设置界面中文
1. 修改 Settings/Interface/Skin/Fonts 为 Arial Based
2. 修改 Settings/Interface/Regional/Language 为 Chinese(Simple)

# 第三方刮削器
Kodi 自带的刮削器受到网络限制。推荐使用第三方刮削器 tinyMediaManager，软件默认的刮削源为 The Movie Database（TMDB）。TMDB 是一个公开的影视数据库。
> The Movie Database (TMDB) is a community built movie and TV database. Every piece of data has been added by our amazing community dating back to 2008. TMDB's strong international focus and breadth of data is largely unmatched and something we're incredibly proud of. Put simply, we live and breathe community and that's precisely what makes us different.

# 媒体命名规则
- 建议所有的媒体文件路径仅包含英文、数字和半角符号。
- 建议其中的半角符号仅为空格、圆括号、连字符和句点。

## 目录层级示例
```plaintext
<root>
    <movie> (<year>)
        <movie>.<ext>
    <tvshow> (<year>)
        Season 00
            <tvshow> S00E01.<ext>
        Season 01
            <tvshow> S01E01.<ext>
            <tvshow> S01E02 - 720P.<ext>
            <tvshow> S01E02 - 1080P.<ext>
            <tvshow> S01E03E04.<ext>
```
其中，Season 00 表示特典，在 Kodi 中以 Specials 显示。
> Specials are named as season 00 and start at E01, e.g. S00E01.

# 参考链接
- [kodi如何设置简体中文](https://www.bilibili.com/video/BV1CL411G7ZZ/?share_source=copy_web&vd_source=b35357ac0ea0f85de9853e12d095f019)
- [About TMDB — The Movie Database (TMDB)](https://www.themoviedb.org/about)
- [Naming video files/Episodes - Official Kodi Wiki](https://kodi.wiki/view/Naming_video_files/Episodes)
