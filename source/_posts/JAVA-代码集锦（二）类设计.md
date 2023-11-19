---
title: JAVA 代码集锦（二）类设计
date: 2023-11-19 14:29:22
updated:
tags:
  - Java
categories:
  - 沉淀
  - 集锦
keywords:
  - 数列求和
  - 重叠矩形
  - 比较器
  - 简单工厂
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
# 数列求和
```java
// 数列求和
// 2/1, 3/2, 5/3, 8/5, 13/8, 21/13, ...
// 从键盘输入项数 n, n 不小于 1, 输出结果保留 6 位小数

// 测试数据
// （输入）
// 20
// （输出）
// 32.660261

import java.util.Scanner;

public class Main {

  // 定义斐波那契数列
  // 1, 1, 2, 3, 5, 8, 13, ...
  // k 的值不小于 3
  public static double fibonacci(int k) {
    if (k == 1 || k == 2) {
      return 1.0;
    }
    return fibonacci(k - 1) + fibonacci(k - 2);
  }

  // 获取所求数列第 n 项的值
  public static double getValue(int n) {
    int k = n + 2;  // 比如第一项 n = 1, k = 3, fibonacci(k) = 2
    return fibonacci(k) / fibonacci(k - 1);
  }

  public static void main(String[] args) {
    Scanner sin = new Scanner(System.in);
    int max = sin.nextInt();
    double sum = 0;
    for (int i = 1; i <= max; ++i) {
      sum += getValue(i);
    }
    System.out.println(String.format("%.6f", sum));
    sin.close();
  }
}
```

# 重叠矩形
```java
// 矩形重叠
// 设计一个矩形类Rectangle，该类符合以下要求：
// (1) 有四个int型域变量(成员变量)分别表示矩形左上角顶点在平面坐标系中的
//   横坐标x、纵坐标y，矩形宽度widh和矩形高度height（坐标系原点在屏幕左上角，x轴水平向右，y轴垂直向下）；
// (2) 有一个构造方法，当调用它时会初始化并按(x,y,width,height)的格式显示该对象的四个域变量值；
// (3) 有一个isOverlapped 方法，该方法接收一个Rectangle对象，并返回当前对象与接收对象之间是否在同一个坐标系中存在重叠现象。
//   若重叠则返回true，否则返回false;
// (4) 有一个主方法，在主方法中先从键盘输入用于第一个Retangle对象的x、y、width和height值，并用输入的这四个值定义第一对象，
//   然后按同样的方法输入并定义第二对象，接着将第二个对象传递给第一个对象的isOverLapped方法，输出这两个对象之间是否存在重叠现象。

// 测试数据
// （输入）
// 0 0 2 2
// 3 3 2 2
// （输出）
// (0,0,2,2)
// (3,3,2,2)
// false

import java.util.Scanner;

class Rectangle {

  private int x, y, width, height;

  public Rectangle(int x, int y, int width, int height) {
    this.x = x; 
    this.y = y; 
    this.width = width; 
    this.height = height; 
  }

  @Override
  public String toString() {
    return "(" + x + "," + y + "," + width + "," + height + ")";
  }

  // 策略
  // 1. 找到包含这两个矩形的最小矩形 area
  // 2. 分别判断两个矩形宽、高方向尺寸之和是否小于等于 area 宽、高方向的尺寸，如果均否，则两矩形不重叠，否则重叠
  public boolean isOverlapped(Rectangle r) {
    int areaX = Math.min(x, r.x);
    int areaY = Math.min(y, r.y);
    int areaWidth = Math.max(x + width, r.x + r.width) - areaX;
    int areaHeight = Math.max(y + height, r.y + r.height) - areaY;
    return !(width + r.width < areaWidth && height + r.height < areaHeight);
  }
}

public class Main {
  
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    Rectangle r1 = new Rectangle(sc.nextInt(), sc.nextInt(), sc.nextInt(), sc.nextInt());
    Rectangle r2 = new Rectangle(sc.nextInt(), sc.nextInt(), sc.nextInt(), sc.nextInt());
    System.out.println(r1);
    System.out.println(r2);
    System.out.println(r1.isOverlapped(r2));
    sc.close();
  }
}
```

# 比较器
```java
// 已知输入中有若干行产品，每个产品按序包括5种产品信息：
// 产品代码(字符串)、产品名称(字符串)、单价(整型)、数量(整型)和金额(长整型)
// 它们之间用空格分隔，每行一个产品。
// 编写满足以下要求的程序：
// (1) 定义一个产品类，它包含一个产品的五种信息，以及能够构造产品对象、存取产品信息的方法；
// (2) 定义一个主类，该类包括：
// 一个产品域： 其作用是按行存储输入中的所有产品；
// 一个输入方法：其功能是读取输入中的所有产品信息并存储到产品域中；
// 一个输出方法：其功能是按序输出产品域中的产品，每行输出一个产品，每个产品的产品信息之间用一个空格分隔；
// 一个排序方法：其功能是将产品域中的产品按产品名称从小到大排序，若产品名称相同，则按金额从小到大排序，结果仍存储在产品域中；
// 一个主方法：其功能是调用主类中定义的其它方法，实现读取输入中的所有产品并存储到主类对象中，然后进行排序，最后将排序结果输出。

// 测试数据
// （输入）
// 0001 计算思维与人工智能基础 52 10 520
// 0002 Java语言程序设计 53 20 1060
// 0003 HTML5+CSS3+JavaScript+BootStrap网站开发使用技术(第3版) 59 5 295
// 0004 计算思维与人工智能基础 52 5 260
// （输出）
// 0003 HTML5+CSS3+JavaScript+BootStrap网站开发使用技术(第3版) 59 5 295
// 0002 Java语言程序设计 53 20 1060
// 0004 计算思维与人工智能基础 52 5 260
// 0001 计算思维与人工智能基础 52 10 520

import java.util.Scanner;
import java.util.ArrayList;
import java.util.Comparator;

class Product {

  String code, name;
  int price, amount;
  long money;

  public Product(String code, String name, int price, int amount, long money) {
    this.code = code; 
    this.name = name; 
    this.price = price; 
    this.amount = amount; 
    this.money = money;
  }

  public String getName() { return name; }

  public long getMoney() { return money; }

  @Override
  public String toString() { 
    return code + " " + name + " " + price + " " + amount + " " + money; 
  }
}

public class Main {

  public static ArrayList<Product> products = new ArrayList<>();

  public static void commit() {
    Scanner sin = new Scanner(System.in);
    for (int i = 0; i < 4; ++i) {
      products.add(new Product(sin.next(), sin.next(), sin.nextInt(), sin.nextInt(), sin.nextLong()));
    }
    sin.close();
  }

  public static void print() { 
    for (int i = 0; i < 4; ++i) { 
      System.out.println(products.get(i));
    } 
  }

  public static void sort() {
    // 匿名内部类实现
    // product.sort(new Comparator<Product>() {
    //     @Override
    //     public int compare(Product p1, Product p2) {
    //         if (!p1.getName().equals(p2.getName())) {
    //             return p1.getName().compareTo(p2.getName());
    //         }
    //         return (int)(p1.getMoney() - p2.getMoney());
    //     }
    // });

    // Lambda表达式实现
    products.sort((a, b) -> a.getName().equals(b.getName()) ? (int)(a.getMoney() - b.getMoney()) : a.getName().compareTo(b.getName()));
  }

  public static void main(String[] args) {
    commit(); 
    sort(); 
    print();
  }
}
```

# 简单工厂
```java
// 简单工厂
// 编写满足以下要求的程序：
// (1) 定义一个图形接口Shape，它有两个方法：
//     double getArea()：其功能是返回图形的面积
//     double getPerimeter ()：其功能是返回图形的周长
// (2) 定义一个矩形类Rectangle，它实现了Shape接口，另有以下字段和方法：
//     w：字段，double类型，存储矩形的宽；
//     h：字段，double类型，存储矩形的高；
//     Rectangle(double w, double h)：构造方法，分别用w和h初始化矩形的宽和高；
//     String toString()：方法，将矩形类对象转换为形如“w=1.0,h=2.0,perimeter=6.0,area=2.00”格式的字符串，
//     要求图形的面积保留两位小数；
// (3) 定义一个三角形类Triangle，它实现了Shape接口，另有以下字段和方法：
//     a,b,c：字段，均为double类型，分别用于存储三角形的三条边长；
//     Triangle(double a,double b,double c)：构造方法，分别用a,b,c初始化三角形的三条边长；
//     String toString()：方法，将三角形对象转换为形如“a=3.0,b=4.0,c=5.0,perimeter=12.0,area=6.00”格式的字符串，
//     要求图形的面积保留两位小数;
// (4) 定义一个工厂类接口Factory，它只有一个方法：
//     Shape create(double ... param)：用于创建并返回图形对象
// (5) 定义一个矩形工厂类，它实现了Factory接口，当调用来自Factory接口的create方法时将用其参数创建并返回参数所指定宽度和高度的
//     Rectangle对象，如果参数不能满足创建矩形对象的要求则返回null。
// (6) 定义一个三角形工厂类，它实现了Factory接口，当调用来自Factory接口的create方法时将用其参数创建并返回参数所指定三条边长的
//     Triangle对象，如果参数不能满足创建三角形对象的要求则返回null。
// (7) 创建一个具有RECTANGLE和TRIANGLE两个枚举常量值的枚举类型；
// (8) 创建一个测试类，在该类中只有一个主方法，主方法的功能是：首先读取两行数据，每一行的数据之间用空格分隔，其每一行的第一个数据是
//     图形名称（对应枚举常量的字符串），其后是图形的边长数据；然后根据读取的图形名称及其图形数据用工厂类创建相应于图形的对象；最后
//     输出图形转换来的字符串；

// 测试数据：
// （输入）
// RECTANGLE 1.0 2.0
// TRIANGLE 3.0 4.0 5.0 
// （输出）
// w=1.0,h=2.0,perimeter=6.0,area=2.00
// a=3.0,b=4.0,c=5.0,perimeter=12.0,area=6.00

import java.util.Scanner;

interface Shape { 
  double getArea(); 
  double getPerimeter(); 
}

interface Factory {
  Shape create(double ... param); 
}

enum Type {
  RECTANGLE, 
  TRIANGLE
}

class Rectangle implements Shape {

  private double w, h;

  public Rectangle(double w, double h) { 
    this.w = w; 
    this.h = h; 
  }

  @Override
  public String toString() {
    return "w=" + w + ",h=" + h + ",perimeter=" + getPerimeter() + ",area=" + String.format("%.2f", getArea());
  }

  @Override
  public double getPerimeter() { return 2 * (w + h); }

  @Override
  public double getArea() { return w * h; }
}

class Triangle implements Shape {

  private double a, b, c;

  public Triangle(double a, double b, double c) { 
    this.a = a; 
    this.b = b; 
    this.c = c; 
  }

  @Override
  public String toString() {
    return "a=" + a + ",b=" + b + ",c=" + c + ",perimeter=" + getPerimeter() + ",area=" + String.format("%.2f", getArea());
  }

  @Override
  public double getPerimeter() { return a + b + c; }

  @Override
  public double getArea() {
    double p = (a + b + c) / 2.0;
    return Math.sqrt(p * (p - a) * (p - b) * (p - c));  // 海伦公式
  }
}

class RectangleFactory implements Factory {

  @Override
  public Rectangle create(double ... param) {
    return param.length == 2 ? new Rectangle(param[0], param[1]) : null;
  }
}

class TriangleFactory implements Factory {

  @Override
  public Triangle create(double ... param) {
    return param.length == 3 ? new Triangle(param[0], param[1], param[2]) : null;
  }
}

public class Test {

  public static void main(String[] args) {
    Scanner sin = new Scanner(System.in);
    Rectangle r = null;
    Triangle a = null;
    for (int i = 0; i < 2; ++i) {
      String type = sin.next();
      if (type.equals(Type.RECTANGLE.name())) {
        r = new RectangleFactory().create(sin.nextDouble(), sin.nextDouble());
      }
      if (type.equals(Type.TRIANGLE.name())) {
        a = new TriangleFactory().create(sin.nextDouble(), sin.nextDouble(), sin.nextDouble());
      }
    }
    sin.close();
    System.out.println(r);
    System.out.println(a);
  }
}
```
