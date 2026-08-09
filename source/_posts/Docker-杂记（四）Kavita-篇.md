---
title: Docker 杂记（四）Kavita 篇
date: 2026-08-09 12:46:19
updated:
tags:
  - Docker
  - Kavita
categories:
  - 沉淀
  - 手记
keywords:
  - Docker
  - Kavita
  - 容器
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
Kavita 是一个开源的图书管理系统，提供了一个简单易用的界面，可用于收藏漫画作品的整理。

根据官方指导的部署指令[^1]，作 Compose 配置如下：

```yaml
services:
  kavita:
    image: jvmilazz0/kavita:latest
    container_name: kavita
    volumes:
      - /home/kgl/manga:/manga
      - ./data:/kavita/config
    environment:
      - TZ=Asia/Shanghai
    ports:
      - "15000:5000"
    restart: unless-stopped
```

其中，漫画目录应组织为 `manga/漫画名/漫画名-v卷序号.cbz` 之类的格式[^2]。

另外，建议在所有的漫画归档文件（如 CBZ）中嵌入 ComicInfo.xml 元数据文件[^3]以方便管理。

[^1]: [Kavita Wiki - Docker Install](https://wiki.kavitareader.com/installation/docker/)
[^2]: [Kavita Wiki - Library Scanner](https://wiki.kavitareader.com/guides/scanner/)
[^3]: [Kavita Wiki - Comics and Manga Metadata](https://wiki.kavitareader.com/guides/metadata/comics/)
