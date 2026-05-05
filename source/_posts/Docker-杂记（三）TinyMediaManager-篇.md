---
title: Docker 杂记（三）TinyMediaManager 篇
date: 2026-05-05 15:29:05
updated:
tags:
  - Docker
  - TinyMediaManager
  - Scraper
categories:
  - 沉淀
  - 手记
keywords:
  - Docker
  - 容器
  - TinyMediaManager
  - Scraper
  - 刮削器
  - Jellyfin
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
TinyMediaManager 是一款知名的影视刮削工具，用于从各大影视数据库获取电影或剧集信息，并提供便捷的本地资源管理功能。
官方提供 Docker 镜像支持：[Docker - tinymediamanager](https://www.tinymediamanager.org/docs/docker)

家里 NAS 上使用的 Docker Compose 配置如下：

```yaml
version: "2.1"
services:
  tinymediamanager:
    image: tinymediamanager/tinymediamanager:latest
    container_name: tinymediamanager
    environment:
      - USER_ID=1000
      - GROUP_ID=10
      - ALLOW_DIRECT_VNC=true
      - LC_ALL=en_US.UTF-8 # force UTF8
      - LANG=en_US.UTF-8   # force UTF8
      - PASSWORD=123456 
    volumes:
      - ./data:/data
      - /home/kgl/media:/media
      - /home/kgl/addons:/app/addons
    ports:
      - 4000:4000 # Webinterface
      - 5900:5900 # VNC port      
    restart: unless-stopped
    logging: # restrict log size
      driver: "json-file"
      options:
        max-file: "5"   # number of files or file count
        max-size: "20m" # file size   
```

其中，`USER_ID` 和 `GROUP_ID` 的值要与当前用户一致，`PASSWORD` 需要设置。

可以看到我的电影和剧集都放在家文件夹的 `media` 目录下，另一个目录 `addons` 可以用来存放 TinyMediaManager 的插件。
除此之外，官方还提供配置高级命令的卷挂载选项。

浏览器访问 NAS 的 4000 端口，连接 NoVNC，使用刚才设置的密码进行登陆，就能使用 TinyMediaManager 了。
初次使用 TinyMediaManager 需要完成向导配置，需要粘贴 TMDB API Key 时，得先粘贴到 NoVNC 左侧工具栏的剪切板上。
申请 TMDB API Key 的教程可见：[【教程】如何申请TMDB（The Movie Database）API 密钥](https://www.ugnas.com/tutorial-detail/id-81.html)

个人的 TinyMediaManager 配置规则包括：

- 开启元数据刮削
  - NFO 格式为 Jellyfin
  - 首选语言：中文（zh-CN）-> 英文（en-US）-> 源语言
- NFO 文件名
  - 电影
    - movie
  - 剧集
    - tvshow
    - season
    - <剧集文件名>
- 开启艺术图片刮削
  - 首选语言：源语言 -> 英文（en-US）-> 中文（zh-CN）-> 其他语言
- 艺术图片文件名
  - 电影
    - 海报：poster
    - 同人图：fanart
    - 缩略图：thumb
    - 其他：banner，discart，clearlogo，clearart，keyart
  - 剧集
    - 海报：poster，<季文件夹名>/seasonXX-poster
    - 同人图：fanart，<季文件夹名>/seasonXX-fanart
    - 缩略图：thumb，<季文件夹名>/seasonXX-thumb，<剧集文件名>-thumb
    - 横幅图：banner，<季文件夹名>/seasonXX-banner
    - 其他：characterart，discart，clearlogo，clearart，keyart
- 关闭“预告片”和“字幕”刮削
- 重命名规则
  - 电影
    - 电影文件夹名：${englishTitle} (${year}) [tmdbid-${showTmdb}]
    - 电影文件名：${englishTitle}
    - 使用 ASCII 字符代替非 ASCII 字符
  - 剧集
    - 剧集文件夹名：${showEnglishTitle} (${showYear}) [tmdbid-${showTmdb}]
    - 季文件夹名：Season ${seasonNr2}
    - 剧集文件名：S${seasonNr2}E${episodeNr2}
    - 使用 ASCII 字符代替非 ASCII 字符
- 后期处理
  - 电影
    - FFmpeg：/data/addons/ffmpeg -y -i poster.webp -q:v 2 poster.jpg
    - Rename: /usr/bin/rename.ul .webp .jpg *.webp
