---
title: Docker 杂记（一）Node 篇
date: 2025-12-13 07:24:50
updated:
tags:
  - Docker
  - Node
categories:
  - 沉淀
  - 手记
keywords:
  - Docker
  - Node
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
> 终于想起了我那吃灰的博客……

双 11 购入了一台 NAS，11 月份一直在折腾这东西，这两天想着能不能用 Docker 把我的 Hexo 也弄到 NAS 上管理。

于是我先在自己 Windows 11 PC 上尝试了一下，也算是有了一次配 Docker 环境的经验。

# Docker Desktop 安装
选择官方提供的安装包链接进行下载和安装即可：[Windows | Docker Docs](https://docs.docker.com/desktop/setup/install/windows-install/)。


# 更换 Docker Hub 源
打开 Docker Desktop，选择免登录继续。

然后，在软件右上角设置里，编辑 Docker Engine 的 json 内容如下：
```json
{
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "experimental": false,
  "registry-mirrors": [
    "https://docker.1ms.run"
  ]
}
```

其中的 `https://docker.1ms.run` 是加速源。可以 Bing 搜索“Docker 镜像”或“Docker 加速”获取更多源。

点击应用并重启。

在软件右下角打开终端，输入 `docker info` 命令能够查看到刚刚添加的源。

# 下载 Node.js 镜像
遵循官方指南 [Node.js — 下载 Node.js®](https://nodejs.org/zh-cn/download) 通过 `docker pull` 指令下载。

我一直用的是 `20` 的 `LTS`，所以我的指令是：
```bash
docker pull node:20-alpine
```

# 运行容器
这里先给出 `docker run` 的命令：
```bash
docker run -it -d --name node -p 4000:4000 -v C:\Users\Administrator\Documents\Repos\kongolou.gitlab.io-main:/root/blog node:20-alpine
```
参数解释如下：
- `-it`：开启输入功能并连接伪终端
- `-d`：后台运行容器
- `--name`：为容器指定一个名称
- `-p`：端口映射，格式为：宿主机端口:容器端口 。
- `-v`：工作目录映射。形式为：宿主机路径:容器路径。

可以看出来，我已经把 GitLab 上的博客拉到了本地的 `C:\Users\Administrator\Documents\Repos\kongolou.gitlab.io-main` 目录。而映射 `4000` 端口是为了方便 `hexo server` 进行预览。

# 配置 Hexo 环境
在 Docker Desktop 的容量列表中选择刚刚新建的 `node` 容器，打开 `Exec` 伪终端，安装官方教程 [Hexo](https://hexo.io/zh-cn/index.html) 配置 Hexo。
```bash
cd /root/blog
npm install -g hexo-cli
npm install
```

接下来就又可以愉快地写作了~
```bash
hexo n "Docker 杂记（一）Node 篇"
hexo g && hexo s
hexo cl
hexo d
```

# 关于容器的停止与删除
注意删除容器后，Hexo 环境也随之删除。

不使用 Node 时，停止 `node` 容器即可。
