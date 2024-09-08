---
title: NVS 杂记
date: 2024-09-08 17:14:48
updated: 
tags:
  - NVS
categories:
  - 沉淀
  - 手记
keywords: 
  - NVS
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
# NVS 下载与安装

Windows 离线安装：详见官方项目 `https://github.com/jasongin/nvs` 的 Release
Windows 在线安装：PowerShell 命令 `winget install jasongin.nvs`

# NVS 换源

Windows 下，NVS 默认的安装路径为 `%USERPROFILE%\AppData\Local\nvs`（自动加入环境变量）。
将其中的 `default.json` 文件备份为 `default - Copy.json` ，修改 `default.json` 内容如下：(换清华源)

```json
{
    "aliases": {
    },
    "remotes": {
        "default": "node",
        "node": "https://mirrors.tuna.tsinghua.edu.cn/nodejs-release/"
    },
    "bootstrap": "node/16.20.2"
}
```

文件夹导航栏输入 `cmd` 打开控制台窗口，安装最新的 Node.js 长期支持版本：

```cmd
nvs add lts
```

# NPM 换源

1. 在控制台中，切换到特定版本的 Node.js ；

```cmd
nvs use lts
```

2. 将该版本的 NPM 源换为镜像源；（这里以淘宝源为例）

```cmd
npm config set registry https://registry.npmmirror.com
```

3. 来到活动目录，安装需要的包；（`--verbose` 选项用于查看安装的详细情况）

```cmd
npm install --verbose
```

4. 通过 NPX 运行包。如：

```cmd
npx hexo g
```

# 参考资料

- [nodejs-release | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/nodejs-release/)
- [npm install运行卡住不动解决办法-CSDN博客](https://blog.csdn.net/qq_15975233/article/details/141815568)
