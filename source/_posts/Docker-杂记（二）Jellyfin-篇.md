---
title: Docker 杂记（二）Jellyfin 篇
date: 2026-02-02 21:54:30
updated:
tags:
  - Docker
  - Jellyfin
categories:
  - 沉淀
  - 手记
keywords:
  - Docker
  - Jellyfin
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
Jellyfin 官方给出的 [Jellyfin Server Docker](https://jellyfin.org/downloads/docker/) 安装建议如下:
> # Docker
> Run Jellyfin in Docker. Example commands store data in `/srv/jellyfin` and assume your media is stored under `/media`.
> ```bash
> docker pull jellyfin/jellyfin:latest  # or docker pull ghcr.io/jellyfin/jellyfin:latest
> mkdir -p /srv/jellyfin/{config,cache}
> docker run -d -v /srv/jellyfin/config:/config -v /srv/jellyfin/cache:/cache -v /media:/media --net=host jellyfin/jellyfin:latest
> ```

容器部署主要在第三步。个人不太建议直接使用 `--net=host`，因此，本人对第三步作了一些个性化改进：

```bash
docker run -d --name jellyfin -p 8096:8096 -v C:\Users\Administrator\Videos\Jellyfin\config:/config -v C:\Users\Administrator\Videos\Jellyfin\cache:/cache -v C:\Users\Administrator\Videos\Jellyfin\media:/media jellyfin/jellyfin:latest
9a8ce791abe61cc212198f17630aa68ed21a6604bc17f2eabe6f3f7c96719ece
```

Jellyfin 默认的 HTTP 端口 8096，而 HTTPS 端口是 8920，但 HTTPS 默认不启用，因此，只需要映射 8096 端口即可。

可以看到，本人的本地目录结构为：

```
C:\Users\Administrator\Videos\Jellyfin\
├─cache
├─config
└─media
```

对应 Jellyfin 监控的目录结构为：
```
/
/cache
/config
/media
```

其中，`media` 用于存储媒体资源。
