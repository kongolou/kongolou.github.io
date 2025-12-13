---
title: Java 代码集锦（一）方法设计
date: 2023-11-17 10:21:36
updated:
tags:
  - Java
categories:
  - 沉淀
  - 集锦
keywords:
  - 九九乘法表
  - 杨辉三角
  - 空心金字塔
  - 空心菱形
  - 猜数游戏
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
# 九九乘法表
```java
/**
 * MultiplicationTable.java
 * 
 * 打印九九乘法表
 * 1x1=1
 * 2x1=2   2x2=4
 * 3x1=3   3x2=6   3x3=9
 * 4x1=4   4x2=8   4x3=12  4x4=16
 * 5x1=5   5x2=10  5x3=15  5x4=20  5x5=25
 * 6x1=6   6x2=12  6x3=18  6x4=24  6x5=30  6x6=36
 * 7x1=7   7x2=14  7x3=21  7x4=28  7x5=35  7x6=42  7x7=49
 * 8x1=8   8x2=16  8x3=24  8x4=32  8x5=40  8x6=48  8x7=56  8x8=64
 * 9x1=9   9x2=18  9x3=27  9x4=36  9x5=45  9x6=54  9x7=63  9x8=72  9x9=81
 */
public class MultiplicationTable{
  public static void main(String[] args) {
    for (int i = 1; i < 10; i++) {
      for (int j = 1; j < i + 1; j++) {
        System.out.print(i + "*" + j + "=" + (i * j) + "\t");
      }
      System.out.println();
    }
  }
}
```

# 杨辉三角
```java
/**
 * YangHuiTriangle.java
 * 
 * 打印杨辉三角
 * Please enter layers of the Yang Hui Triangle:
 * 6
 * Result:
 * 1
 * 1       1
 * 1       2       1
 * 1       3       3       1
 * 1       4       6       4       1
 * 1       5       10      10      5       1
 */
import java.util.Scanner; // support for Scanner

public class YangHuiTriangle {

  public static void main(String[] args) {
    // 获取杨辉三角层数
    Scanner myScanner = new Scanner(System.in);
    System.out.println("Please enter layers of the Yang Hui Triangle:");
    int layers = myScanner.nextInt();
    myScanner.close();
    // 生成杨辉三角
    int[][] yangHui = new int[layers][];
    for (int i = 0; i < layers; i++) {
      // 第 i 行有 i + 1 个元素
      yangHui[i] = new int[i + 1];
      for (int j = 0; j < i + 1; j++) {
        if (j == 0 || j == i) {
          // 首尾是 1
          yangHui[i][j] = 1;
        } else {
          // 每一项为上面的项与上面项前一项的和，如
          // .
          // 1       1
          // .       2       .
          // 即 2 = 1 + 1
          yangHui[i][j] = yangHui[i - 1][j] + yangHui[i - 1][j - 1];
        }
        // 从这里可以看出杨辉三角存在递归性质
        // 每一层的数列其实是上一层错位相加得到的，如下
        // Layer2: 1       1
        // +       Layer2: 1       1
        // --------------------------------
        // Layer3: 1       2       1
      }
    }
    // 打印杨辉三角
    System.out.println("Result:");
    for (int i = 0; i < layers; i++) {
      for (int j = 0; j < i + 1; j++) {
        System.out.print(yangHui[i][j] + "\t");
      }
      System.out.println();
    }
  }

  // 这里给出获取杨辉三角每一层的递归方法参考
  @SuppressWarnings("unused")
  public static int[] getLayer(int layer) {
    if (layer == 1) {
      return new int[] { 1 };
    }
    int[] previousLayer = getLayer(layer - 1);
    int[] currentLayer = new int[layer];
    // 首尾是 1
    currentLayer[0] = 1;
    currentLayer[layer - 1] = 1;
    // 中间部分为上一层错位相加
    for (int i = 1; i < layer - 1; i++) {
      currentLayer[i] = previousLayer[i] + previousLayer[i - 1]; 
    }
    return currentLayer;
  }
}
```

# 空心金字塔
```java
/**
 * HollowPyramid.java
 * 
 * 打印空心金字塔
 * Levels of the HollowPyramid: 5
 *     *
 *    * *
 *   *   *
 *  *     *
 * *********
 */
import java.util.Scanner;
public class HollowPyramid{
  public static void main(String[] args) {
    // 获取空心金字塔的层数
    Scanner myScanner = new Scanner(System.in);
    System.out.print("Levels of the HollowPyramid: ");
    int level = myScanner.nextInt();
    myScanner.close();
    // 打印空心金字塔
    for (int i = 1; i <= level; i++) {
      // 打印左边的空格
      for (int k = 1; k <= level - i; k++) {
        System.out.print(" ");
      }
      // 打印金字塔边界
      for (int j = 1; j <= 2 * i - 1; j++) {
        // 判断是首尾或底部就打印星号，否则打印空格
        if (j == 1 || j == 2 * i - 1 || i == level) {
          System.out.print("*");
        } else {
          System.out.print(" ");
        }
      }
      // 一行结束
      System.out.println();
    }
  }
}
```

# 空心菱形
```java
/**
 * HollowDiamond.java
 * 
 * 打印空心菱形
 * Please enter an odd number: 6
 * Please enter an odd number: 5
 *   *
 *  * *
 * *   *
 *  * *
 *   *
 */
import java.util.Scanner;
public class HollowDiamond{
  public static void main(String[] args) {
    // 获取空心菱形的层数，为了好看，需要是奇数
    Scanner myScanner = new Scanner(System.in);
    int maxLayer = 0;
    while(maxLayer % 2 == 0) {
      System.out.print("Please enter an odd number:");
      maxLayer = myScanner.nextInt();
    }
    myScanner.close();
    // 获取中间的一层（整数除法）
    int thickLayer = maxLayer / 2 + 1;
    // 打印上半部分
    for (int i = 1; i <= thickLayer; i++) {
      for (int k = 1; k <= thickLayer - i; k++) {
        System.out.print(" ");
      }
      for (int j = 1; j <= 2 * i - 1; j++) {
        if (j == 1 || j == 2 * i - 1) {
          System.out.print("*");
        } else {
          System.out.print(" ");
        }
      }
      System.out.println();
    }
    // 打印下半部分
    for (int i = thickLayer + 1; i <= maxLayer; i++) {
      for (int k = 1; k <= i - thickLayer; k++) {
        System.out.print(" ");
      }
      for (int j = 1; j <= 2 * (maxLayer - (i - 1)) - 1; j++) {
        if (j == 1 || j == 2 * (maxLayer - (i - 1)) - 1) {
          System.out.print("*");
        } else {
          System.out.print(" ");
        }
      }
      System.out.println();
    }
  }
}
```

# 猜数游戏
## 玩家
```java
/**
 * GuessGameDemo/Player.java
 * 
 * 玩家类
 */
public class Player{
  int number = 0;
  public void guess(){
    number = (int)(Math.random() * 10);
    System.out.println("I'm guessing " + number);
  }
}
```

## 玩法
```java
/**
 * GuessGameDemo/GuessGame.java
 * 
 * 玩法类
 */
public class GuessGame{
  Player p1, p2, p3;
  public void startGame(){
    p1 = new Player();
    p2 = new Player();
    p3 = new Player();

    int guessp1 = 0;
    int guessp2 = 0;
    int guessp3 = 0;

    boolean p1isRight = false;
    boolean p2isRight = false;
    boolean p3isRight = false;

    int targetNumber = (int)(Math.random() * 10);
    System.out.println("I'm thinking a number between 0 and 9...");

    while(true){
      System.out.println("Number to guess is " + targetNumber);

      p1.guess();
      p2.guess();
      p3.guess();

      guessp1 = p1.number;
      System.out.println("Player one guessed " + guessp1);
      guessp2 = p2.number;
      System.out.println("Player two guessed " + guessp2);
      guessp3 = p3.number;
      System.out.println("Player three guessed " + guessp3);

      if (guessp1 == targetNumber) {
        p1isRight = true;
      }
      if (guessp2 == targetNumber) {
        p2isRight = true;
      }
      if (guessp3 == targetNumber) {
        p3isRight = true;
      }

      if (p1isRight || p2isRight || p3isRight) {
        System.out.println("We have a winner!");
        System.out.println("Player one got it right? " + p1isRight);
        System.out.println("Player two got it right? " + p2isRight);
        System.out.println("Player three got it right? " + p3isRight);
        System.out.println("Game is over.");
        break;
      } else {
        System.out.println("Players will have to try again.");
      }
    } // end while
  }
}
```

## 启动器
```java
/**
 * GuessGameDemo/GameLauncher.java
 * 
 * 启动器类
 */
public class GameLauncher{
  public static void main(String[] args){
    new GuessGame().startGame();
  }
}
```

## 测试
```plaintext
I'm thinking a number between 0 and 9...
Number to guess is 3
I'm guessing 0
I'm guessing 0
I'm guessing 1
Player one guessed 0
Player two guessed 0
Player three guessed 1
Players will have to try again.
Number to guess is 3
I'm guessing 5
I'm guessing 5
I'm guessing 6
Player one guessed 5
Player two guessed 5
Player three guessed 6
Players will have to try again.
Number to guess is 3
I'm guessing 4
I'm guessing 5
I'm guessing 4
Player one guessed 4
Player two guessed 5
Player three guessed 4
Players will have to try again.
Number to guess is 3
I'm guessing 4
I'm guessing 7
I'm guessing 2
Player one guessed 4
Player two guessed 7
Player three guessed 2
Players will have to try again.
Number to guess is 3
I'm guessing 4
I'm guessing 5
I'm guessing 3
Player one guessed 4
Player two guessed 5
Player three guessed 3
We have a winner!
Player one got it right? false
Player two got it right? false
Player three got it right? true
Game is over.
```

# 参考教程
- [【零基础 快速学Java】韩顺平 零基础30天学会Java](https://www.bilibili.com/video/BV1fh411y7R8/?spm_id_from=333.999.0.0&vd_source=21330d56e1fc273feed05474d179b07c)
- 《Head First Java》
