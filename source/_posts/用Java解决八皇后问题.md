---
title: 用 Java 解决八皇后问题
date: 2023-03-26 18:53:30
updated:
tags:
- code
- java
categories:
- 知识分享
keywords:
- java
- 八皇后
description: 回溯法经典案例。
top_img:
comments:
cover: https://cdn.cdnjson.com/tvax3.sinaimg.cn//large/0072Vf1pgy1foxk41fsxtj31hc0u0k3f.jpg
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

```java
// 作者：kongolou
// 最后一次更新日期：2022-09-27 14:12:00
// 
// 问题描述：在一个棋盘上摆放 8 个皇后，使其不能相互攻击（不在同一行、列和斜线上），问有多少种摆法？
// 思路分析：
//     1. 一种摆法可以被一个一维数组表示，数组下标对应横坐标，而数组元素对应纵坐标，即 f[x] = y 。
//     2. 不在同一行和同一列是指，每个皇后的 x 坐标不能相同，且 y 坐标不能相同。
//     3. 不在同一斜线上是指，任意两个皇后连线的斜率不能是 1 或 -1，即 x 增量与 y 增量不相等且不互为相反数。
//     4. 最好的策略应该是逐列寻找，即从 x = 0 找到 x = 7 。
//     5. 在每一列中寻找时，需要与前几列的每个皇后相比较。
// 一种摆法：0 4 7 5 2 6 1 3
// 以下代码试图找出一种摆法。

package com.kongolou.eightqueens;

public class Main {
    public static void main(String[] args) {
    Methods methods = new Methods();
    methods.backtracking();
    }
}

class Methods {

    private int[] queens = new int[8]; // queens[i] = 0
    private int currentLine = 1; // 我们从第二列开始，我们相信一定可以找到一组解

    protected void backtracking() {
        if (currentLine == 8) {
            for (int i = 0; i < 8; i++) {
                System.out.print(queens[i] + " ");
            }
            return; // 递归走到了尽头，该回去了
        } else {
            for (int i = 0; i < currentLine; i++) {
                if (queens[currentLine] == 8) { // 越界，说明这一列无解
                    // if (currentLine == -1) { // 糟糕，列数也越界了
                    // System.out.println("404 not found."); // 说明无论如何也找不到解
                    // return;
                    // }
                    // 但是我们比较自信，相信肯定有解，不需要这一步判断
                    queens[currentLine] = 0; // 实验结束，整理仪器
                    currentLine--; // 来到上一列
                    queens[currentLine]++;
                    backtracking(); // 注意到上一列不是从 0 开始
                    return; // 返回到上一级 backtracking() 处
                } else {
                    int dx = currentLine - i;
                    int dy = queens[currentLine] - queens[i];
                    if (dy == 0 || dy == dx || dy == -dx) { // 与之前的皇后相克
                        queens[currentLine]++; // 来到该列下一个位置
                        i = -1; // 依旧从第一个皇后开始判断
                    }
                }
            }
            currentLine++; // 来到下一列
            backtracking();
            return; // 返回到上一级 backtracking() 处
        }
    }

}
```
