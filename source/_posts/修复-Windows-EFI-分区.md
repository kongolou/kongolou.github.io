---
title: 修复 Windows EFI 分区
date: 2026-02-03 22:00:36
updated:
tags:
  - Windows
categories:
  - 沉淀
  - 手记
keywords:
  - EFI
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
故事是我在安装 Windows + Ubuntu 双系统的时候，后知后觉安装的 Ubuntu 的版本太新了，一怒之下在终端中执行了传说中的指令
```bash
sudo rm -rf /*
```
我承认这是一个愚蠢的决定，因为这个操作意外删除了 Windows 的 EFI 分区，直接导致我的 Windows 无法被正确引导启动，然后我便开始了漫长的查文档之旅，只是为了修复这个该死的 EFI 分区……

{% note warning %}
需要事先准备一块装载 Windows 镜像的 U 盘
{% endnote %}

1. 进入 BIOS，选择从 U 盘启动
2. 随后加载 Windows 镜像，设置完语言后，选择修复计算机
3. 进入疑难解答，打开命令提示符，依次输入如下指令

```cmd
# 为 EFI 分区分配临时盘符
diskpart        # 这里用到的是 diskpart 工具
list volume     # 确认 EFI 分区卷号（通常为 100 到 500 MB 的 FAT32 分区）
select volume 2 # 这一步将 2 替换为实际的 EFI 分区卷号（EFI_BOOT）
assign letter=S # 分配临时盘符 S
exit            # 退出 diskpart 交互环境

# 通过 bcdboot 工具重建引导
bcdboot C:\Windows /s S: /f UEFI /l zh-cn

# 移除临时盘符
diskpart
select volume 2
remove letter=S
exit
```

4. 重启，立刻拔掉 U 盘，此时应正确引导进入 Windows
