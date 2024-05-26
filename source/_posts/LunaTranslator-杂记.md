---
title: LunaTranslator 杂记
date: 2024-05-26 17:08:32
updated:
tags:
  - LunaTranslator
categories:
  - 沉淀
  - 手记
keywords:
  - LunaTranslator
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
LunaTranslator 是一款 Galgame 翻译器，支持剪贴板、OCR、HOOK 等。与团子翻译器（DangoTranslator）类似，LunaTranslator 的主要功能也是体现在文字提取和翻译处理这两个模块。出于个人隐私和商业在线翻译存在次数限制的考虑，本人采用的是 TGW+ 的丐版配置。

- [GitHub - HIllya51/LunaTranslator: Galgame翻译器，支持剪贴板、OCR、HOOK等。Visual Novel translate tool , support clipboard / OCR/ HOOK](https://github.com/HIllya51/LunaTranslator)
- [GitHub - oobabooga/text-generation-webui: A Gradio web UI for Large Language Models. Supports transformers, GPTQ, AWQ, EXL2, llama.cpp (GGUF), Llama models.](https://github.com/oobabooga/text-generation-webui)

TGW，全称 Text Generation WebUI，本质上是一个大语言模型。TGW+ 即翻译模块采用 text-generation-webui，配合多种文字提取方式的软件配置方案。这里要感谢 B 站 up 主 \_Smzh\_ 贡献的模型资源和使用教程。

- [新版text-generation-webui一键懒人包（解压即用）](https://www.bilibili.com/video/BV1Te411U7me/?share_source=copy_web&vd_source=b35357ac0ea0f85de9853e12d095f019)
- [实时AI汉化翻译工具，LunaTranslator，几乎完美的内嵌翻译游戏体验，TGW翻译源对接介绍。](https://www.bilibili.com/video/BV1au4m1A7g7/?share_source=copy_web&vd_source=b35357ac0ea0f85de9853e12d095f019)

以下为本人在 Windows 系统下配置 LunaTranslator 的参考步骤：（HOOK+TGW）
1. 右键点击“此电脑”打开“计算机管理”，查看显卡并更新驱动。（如 NVIDIA GeForce RTX 2060 6G）
2. 新建文件夹 Translator。下载 LunaTranslator、text-generation-webui，分别解压并放置到该目录下。下载对应的语言模型（如 Sakura 1b8 exl2），解压并放置在 text-generation-webui 的 models 目录下。
3. （关闭杀软后）运行 TGW 程序（start_windows.bat）。在控制面版中，选择并加载（Load）Model（如 Sakura 1b8 exl2）和对应的 Parameters/Instruction template（如 Sakura 模型对应的 ChatML）。按照需要调整相应参数。（如 Sakura 1b8 exl2 模型需设置 max_seq_len=2048）
4. 运行 LunaTranslator。打开设置，勾选“翻译设置/离线翻译”中的“TGW语言模型”，点击后面的按钮进一步设置 instruction template 为 ChatML。（对应 Sakura 模型）
5. 运行游戏，在 LunaTranslator “文本输入”设置中选择源为“HOOK”，点击“选择游戏”找到对应的游戏进程。开始游戏。
6. 当游戏底部对话框中出现第一句话时，点击 LunaTranslator 工具栏中的“选择文本”按钮，勾选需要显示的文本，取消所有的内嵌翻译。
7. 在 LunaTranslator “文本处理”设置中，调整“文本预处理”的方法，运行并观测底部示例，直至一句话的翻译能符合预期。
8. 退出时，依次关闭游戏、LunaTranslator、TGW 网页和控制台。
