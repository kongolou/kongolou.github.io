---
title: Java 代码集锦（三）文件拷贝
date: 2023-11-19 15:38:16
updated:
tags:
  - Java
categories:
  - 沉淀
  - 集锦
keywords:
  - 文件拷贝
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
Java 中所有 io 类均继承自 Reader、Writer、InputStream、OutputStream 这四个抽象类。
其中，继承自 Reader、Writer 的类为字符处理流，继承自 InputStream、OutputStream 的类为字节处理流。

假设已存在文件 test.txt，现要将其备份到 text_bak.txt 文件中。
在 Java 中，文件拷贝（文件复制）有以下几种方式。

# 方案一：单字符传输

```java
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Main {
  public static void main(String[] args) {
    try (FileReader fr = new FileReader("test.txt"); FileWriter fw = new FileWriter("test_bak.txt")) {
      for (int c; (c = fr.read()) != -1; fw.write(c)) {}
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```

# 方案二：多字符传输（数组）
```java
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Main {
  public static void main(String[] args) {
    try (FileReader fr = new FileReader("test.txt"); FileWriter fw = new FileWriter("test_bak.txt")) {
      char[] buf = new char[1024];
      for (int c; (c = fr.read(buf)) != -1; fw.write(buf, 0, c)) {}
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```

# 方案三：多字符传输（缓冲区）
```java
import java.io.FileReader;
import java.io.FileWriter;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;

public class Main {
  public static void main(String[] args) {
    try (BufferedReader br = new BufferedReader(new FileReader("test.txt")); 
        BufferedWriter bw = new BufferedWriter(new FileWriter("test_bak.txt"))) {
      for (String line; (line = br.readLine()) != null; bw.write(line), bw.newLine()) {}
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```

# 方案四：单字节传输
```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class Main {
  public static void main(String[] args) {
    try (FileInputStream fis = new FileInputStream("test.txt"); 
        FileOutputStream fos = new FileOutputStream("test_bak.txt")) {
      for (int b; (b = fis.read()) != -1; fos.write(b)) {}
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```

# 方案五：多字节传输（数组）
```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class Main {
  public static void main(String[] args) {
    try (FileInputStream fis = new FileInputStream("test.txt"); 
        FileOutputStream fos = new FileOutputStream("test_bak.txt")) {
      byte[] buf = new byte[1024];
      for (int len; (len = fis.read(buf)) != -1; fos.write(buf, 0, len)) {}
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```

# 方案六：多字节传输（缓冲区）
```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.IOException;

public class Main {
  public static void main(String[] args) {
    try (BufferedInputStream bis = new BufferedInputStream(new FileInputStream("test.txt"));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("test_bak.txt"))) {
      for (int b; (b = bis.read()) != -1; bos.write(b)) {}
    }
    catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```
