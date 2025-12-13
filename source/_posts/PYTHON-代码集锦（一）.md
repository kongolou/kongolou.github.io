---
title: Python 代码集锦（一）
date: 2023-11-20 10:35:30
updated:
tags:
  - Python
categories:
  - 沉淀
  - 集锦
keywords:
  - 时间格式转换
  - 文件格式转换
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
# 汉诺塔
```python
# 汉诺塔模型
total_levels: int  # 总层数
column_from: list  # 起点
column_to: list  # 终点
column_via: list  # 中转


# 数学归纳法
# n = 1:
# move 1 level from column_a to column_b
# n > 1:
# move (n - 1) levels from column_a to column_c
# move the (n)th level from column_a to column_b
# move the (n - 1) levels from column_c to column_b
def move_hanoi(column_a, column_b, column_c, move_levels):
    global column_from, column_to, column_via
    if move_levels == 1:
        column_b.append(column_a.pop())
        print_hanoi()
        return
    move_hanoi(column_a, column_c, column_b, move_levels - 1)
    move_hanoi(column_a, column_b, column_c, 1)
    move_hanoi(column_c, column_b, column_a, move_levels - 1)


# 打印汉诺塔
def print_hanoi():
    global column_from, column_to, column_via
    print("column_from: ", column_from)
    print("column_to  : ", column_to)
    print("column_via : ", column_via)
    print()


# 初始化模型
total_levels = eval(input("请输入汉诺塔层数："))
column_from = list(range(total_levels))
column_to = []
column_via = []
print_hanoi()
# 展示汉诺塔移动步骤
move_hanoi(column_from, column_to, column_via, total_levels)
print("完成")
```

# 用时间创建 ID 号码
```python
import datetime

# 创建如 20231120103824 格式的 ID
id = datetime.datetime.now().strftime(r"%Y%m%d%H%M%S")
print(id)
```

# CSV 格式文件转换为 DB 格式文件
```python
import pandas as pd
import sqlite3

# 手动创建 test.csv 文件，第一行为标题行
# 转换后可通过 .schema 命令查看
# 读取刚才创建的 test.csv 文件
df = pd.read_csv("test.csv")
# 创建到 test.db 文件的数据库连接，如果没有 test.db 文件则会自动新建
con = sqlite3.connect("test.db")
# 将 test.csv 文件中的数据导入 test.db 文件的 test 表中
# name: 表名
# con: 数据库连接
# if_exists: 存在时，是否替换表，默认 'fail' ，可选 'replace' 和 'append' 
# index: 是否保留索引列，默认 True
# 参考 https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_sql.html
df.to_sql(name="test", con=con, if_exists='replace', index=False)
# 关闭数据库连接
con.close()
```

# HTML 格式文件转换为 PDF 格式文件
```python
from weasyprint import HTML

HTML("test.html").write_pdf("test.pdf")
```

# 画菱形
```python
from PIL import Image, ImageDraw

# 创建一个 100x100 的青色画布
im = Image.new("RGB", (100, 100), "cyan")
# 创建一个画笔
draw = ImageDraw.Draw(im)
# 画一个黄色菱形
draw.polygon([(50, 10), (90, 50), (50, 90), (10, 50)], fill="yellow")
# 保存到 test.png 文件
im.save("test.png")
```
