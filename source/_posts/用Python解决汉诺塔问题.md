---
title: 用 Python 解决汉诺塔问题
date: 2023-03-26 18:31:50
updated:
tags:
- code
- python
categories:
- 知识分享
keywords:
- python
- hanoi
- 汉诺塔
description: 递归的本质是数学归纳法。
top_img:
comments:
cover: https://cdn.cdnjson.com/tvax3.sinaimg.cn//large/0072Vf1pgy1foxkcymwgnj31kw0w0qsx.jpg
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
---

# 代码

```python
# 汉诺塔模型
total_levels = 1
column_from = []
column_to = []
column_via = []


# 数学归纳法
# n = 1:
#   move 1 level from column_a to column_b
# n > 1:
#   move (n - 1) levels from column_a to column_c
#   move the (n)th level from column_a to column_b
#   move the (n - 1) levels from column_c to column_b
def move_hanoi(column_a, column_b, column_c, move_levels):
    if move_levels == 1:
        column_b.append(column_a.pop())
        print(column_from, column_to, column_via)
        return
    else:
        move_hanoi(column_a, column_c, column_b, move_levels - 1)
        move_hanoi(column_a, column_b, column_c, 1)
        move_hanoi(column_c, column_b, column_a, move_levels - 1)


# 期望用户输入一个正整数
def main():
    try:
        global total_levels, column_from
        total_levels = eval(input("请输入汉诺塔层数："))
        column_from = list(range(total_levels))
        print(column_from, column_to, column_via)
        move_hanoi(column_from, column_to, column_via, total_levels)
        print("解算完成，汉诺塔层数：", total_levels)
    except Exception:
        print("期望一个正整数:)")


main()

```
# 参考链接

[教你如何写递归（数学归纳法，干货强推！）](https://www.cnblogs.com/LiCheng-/p/8206444.html)
