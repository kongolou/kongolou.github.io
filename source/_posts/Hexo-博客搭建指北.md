---
title: Hexo 博客搭建指北
date: 2023-07-14 09:43:11
updated:
tags:
  - Hexo
  - blog
categories:
  - 沉淀
  - 指北
keywords:
  - Hexo
  - 博客
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
# 写在前面

关于 Hexo 博客搭建，网上所谓的教程铺天盖地，跟着走了不少弯路，才发现大部分人都是在吹牛逼，最终我决定回到看官方教程的正道上来。

本文写作于 2023 年 7 月 14 日，仅为[官方教程](https://hexo.io/zh-cn/docs/github-pages)的拙劣注解与擅自补充，主要用于记录自己搭建博客的经验历程。如果能对你有所帮助，那就太好了。

# 准备工作-α

## 远程侧

新建 GitHub 仓库，命名为 ~~username~~.github.io，选择不附带 README.md 文件。

## 本地侧

> 不迷路的前提是我们时刻清楚自己身处何方。

我们现在身处你新建的博客目录下，比如是 `blog/`

### 部署 Hexo

安装 Node.js 和 Git 是安装 Hexo 的前置条件。  
在 Archlinux 下，进入 `blog/` 打开终端，依次执行如下命令：  
```bash
$ sudo pacman -S git nodejs npm         # 按照必要的包
$ sudo npm install -g hexo-cli          # 注意 -g 为全局选项，需要 sudo
$ cd blog/                              # 进入你新建的博客目录
$ hexo init                             # 届时 blog/ 目录中将出现相应文件
$ npm install hexo-deployer-git --save  # 安装一键部署工具
$ vim _config.yml                       # 接下来编辑 Hexo 配置文件
```
在 Hexo 配置文件 `_config.yml` 的结尾处找到以下内容并修改：  
```yaml
deploy:
	type: git
	branch: gh-pages
	repo:  # 必填，填博客所在远程仓库的地址
```

### 部署 SSH 连接到 GitHub

#### 创建公钥 [???](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

先检查本地有没有公钥，如果没有就新建一个：  
```bash
$ ls -al ~/.ssh
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```
接下来是著名的“三次回车”：  
```plaintext
Enter a file in which to save the key (~/.ssh/ALGORITHM): [Press enter]
Enter passphrase (empty for no passphrase): [Type a passphrase or press enter]
Enter same passphrase again: [Type the passphrase or press enter]
```

#### 创建私钥 [???](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

```bash
$ cat ~/.ssh/id_ed25519.pub
```
将打印出的信息复制下来，到远程侧创建私钥。最后我们需要验证是否连接成功：  
```bash
$ ssh -T git@github.com
```
如果正确匹配以下 SHA256 指纹，则输入 yes  
```plaintext
The authenticity of host 'github.com (IP ADDRESS)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
Are you sure you want to continue connecting (yes/no)? yes
```
如果看到它和你打招呼，即代表连接成功。

# 项目管理-α

提示：前方发现 git 出没，请拿好[那张图](https://www.runoob.com/git/git-basic-operations.html)。

## 本地侧

### 部署 Git

> 知己知彼，百战不殆。  

首先要明确我们的战略，我们的博客将来要部署在基佬仓库，我们不仅希望其能对外展示，还希望其能托管我们源文件夹下的代码以防日后设备遭遇不测。  

因此我们需要使用双分支，其中，main 分支用于托管代码，而 gh-pages 分支用于展示博客。  

这也是为什么我们需要使用 git 的原因——我们需要基佬仓库帮助我们托管源文件夹下的代码。  

#### Git 配置

出于某种精神洁癖，以下使用 main 作为远程仓库和本地仓库默认分支名称。  
```bash
$ git config --global init.defaultBranch main              # 设置本地仓库默认分支为 main
$ git config --global user.email "useremail@example.com"   # 请替换其中的 useremail...
$ git config --global user.name "username"                 # 请替换其中的 username
```

#### Git 管理

首先在 `blog/` 下部署本地仓库，并与远程仓库取得联系。  
```bash
$ git init                                                              # 本地仓库初始化
$ git remote add origin git@github.com:username/username.github.io.git  # 请替换其中的 username
$ git remote -v                                                         # 展示远程连接
```

接下来我们将对你新建的远程仓库进行第一次推送。  
```bash
$ git add --all                     # 将全部文件载入缓冲区
$ git commit -m "Add source files"  # 提交到本地仓库，-m 是 message 的意思
$ git push -u origin main           # 推送到远程仓库 main -> origin/main
```

### 添加 Workflows

```bash
$ node -v
```
记下 nodejs 的版本号，将来更换版本需要重新记录。  
新建 `blog/.github/workflows/pages.yml` 文件，填入以下内容，将其中的 20 改为你刚才记下的版本号。  
```yaml
name: Pages

on:
  push:
    branches:
      - main # default branch

jobs:
  pages:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js 20.x
        uses: actions/setup-node@v2
        with:
          node-version: "20"
      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

接下来进行第二次推送：  
```bash
$ git add --all
$ git commit -m "Add workflows-pages.yml"
$ git push -u origin main
```

现在远程仓库里已经出现了我们期望的两个分支，接下来还剩最后一步——切换展示分支。

## 远程侧
选择 Repo Settings > Pages > Source，将 main 改为 gh-pages 并 Save，此时博客可通过域名展示。

恭喜！你现在拥有了一个可以公网访问的个人博客网站！而且你没花一分钱！

怎么样，是不是抑制不住内心的激动呢？维护一个博客还有一段漫长的路要走，希望你不忘初心，继续前进！

# 边界曲面的缺失之环-β
比较本地 main 仓库与远程 main 仓库发现，远程 main 仓库缺少 node_modules 文件夹。

这是一种解耦合设计，但是也导致我们在 β 线工作时遇到一些问题。接下来我们解决这些问题。

与 α 线相比，在 β 线里我们开局有一个远程仓库，那是我们在 α 线建立的记忆资料，我们现在要从这个远程仓库中取回记忆。

同样的，我们现在身处一个新建的 `blog/` 目录下。  
```bash
$ sudo pacman -S git nodejs npm
$ git init
$ git remote add origin git@github.com:username/username.github.io  # 注意替换
$ git pull origin main:main
$ npm install
```
最后一行命令是在补齐缺失的依赖包，官方的建议是：  
```bash
$ rm -rf node_modules && npm install --force
```

至此，你可以在 β 线继续开展 hexo 的工作了。

# 参考链接

[ArchWiki | Node.js](https://wiki.archlinuxcn.org/wiki/Node.js)  
[Hexo 文档 | 在 GitHub Pages 上部署 Hexo](https://hexo.io/zh-cn/docs/github-pages)  
[Github 文档 | 关于 SSH](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/about-ssh)  
[菜鸟教程 | Git 教程](https://www.runoob.com/git/git-tutorial.html)  
[fatal: Not a git repository (or any of the parent directories): .git](https://blog.csdn.net/wenb1bai/article/details/89363588)  
[error: src refspec master does not match any.](https://blog.csdn.net/qq_38198952/article/details/82792279)
