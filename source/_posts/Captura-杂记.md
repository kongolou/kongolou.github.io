---
title: Captura 杂记
date: 2024-09-15 19:28:43
updated:
tags:
  - Captura
categories:
  - 沉淀
  - 手记
keywords:
  - Captura
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
{% note warning %}
Captura 项目已停止更新，详见 [End of Life · Issue #570](https://github.com/MathewSachin/Captura/issues/570)。
{% endnote %}

# 安装 FFmpeg

[FFmpeg](https://ffmpeg.org/) 官方网站不提供预构建的二进制文件。但是会推荐两个由第三方构建的包。

## 在线安装

```powershell
winget install ffmpeg --source winget
```

该命令参见 [Builds - CODEX FFMPEG @ gyan.dev](https://www.gyan.dev/ffmpeg/builds/)。

## 离线安装

从 [Releases · BtbN/FFmpeg-Builds](https://github.com/BtbN/FFmpeg-Builds/releases) 获取最新版本的 FFmpeg，一般为 `ffmpeg-master-latest-win64-gpl.zip`。

# 安装 Captura

从 [Releases · MathewSachin/Captura](https://github.com/MathewSachin/Captura/releases) 下载 `Captura-Setup.exe` 进行安装。

## 配置 FFmpeg

如果不将 FFmpeg 添加到环境变量，则需要手动配置 Captura 的 FFmpeg 路径。

推荐 FFmpeg 存放路径：`C:\Program Files\ffmpeg\bin\ffmpeg.exe`。

配置 FFmpeg 的界面还可以配置其他选项参数，如可将录屏文件分辨率改为 `1280x720` 等。

# Q&A

Q：开启音视频录制后，显示内存不足？  
A：勾选分离音频选项。
