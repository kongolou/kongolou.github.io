---
title: C++ 代码集锦
date: 2023-11-07 21:16:49
updated:
tags:
  - C++
categories:
  - 沉淀
  - 集锦
keywords:
  - C++
  - 凯撒密码
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
### 用逻辑表达式实现判断功能

```cpp
#include <iostream>

int main() {
  int a, b;
  std::cin >> a;
  std::cin >> b;

  // if (a > b) {
  //   std::cout << "yes";
  // } else {
  //   std::cout << "no";
  // }
  (a > b) && (std::cout << "yes") || (std::cout << "no");

  return 0;
}
```

### 找给定范围内的素数，每十个输出一行

```cpp
#include <iostream>
#include <cmath>

int main() {
  int start, end;
  std::cout << "请输入左边界整数:  ";
  std::cin >> start;
  std::cout << "请输入右边界整数:  ";
  std::cin >> end;

  int count = 0;
  for (int i = start; i <= end; ++i) {
    bool isPrime = true;
    for (int j = 2; j <= sqrt(i); ++j) {
      if (i % j == 0) {
        isPrime = false;
        break;
      }
    }
    if (isPrime) {
      std::cout << i << " ";
      ++count;
      if (count % 10 == 0) {
        std::cout << std::endl;
        count = 0;
      }
    }
  }

  return 0;
}
```

### 凯撒密码

```cpp
#include <iostream>
#include <string>

int main() {
  std::string str;  // 明文
  std::cin >> str;

  int key;  // 位移
  std::cin >> key;

  std::string result;  // 密文
  for (int i = 0; i < str.size(); i++) {
    if (str[i] >= 'a' && str[i] <= 'z') {
      result += (str[i] - 'a' + key) % 26 + 'a';
    } else if (str[i] >= 'A' && str[i] <= 'Z') {
      result += (str[i] - 'A' + key) % 26 + 'A';
    } else {
      result += str[i];
    }
  }
  std::cout << result << std::endl;

  return 0;
}
```

## 以前的代码

### 解两平面交线方程

```cpp
#include <iostream>

int main() {
  double A1, B1, C1, D1;
  std::cout << "请输入其中一平面参数：";
  std::cin >> A1 >> B1 >> C1 >> D1;
  double A2, B2, C2, D2;
  std::cout << "请输入另一平面参数：";
  std::cin >> A2 >> B2 >> C2 >> D2;
  // 向量叉乘得交线方向
  double A, B, C;
  A = B1 * C2 - B2 * C1;
  B = A2 * C1 - A1 * C2;
  C = A1 * B2 - A2 * B1;
  // 套公式（未知的原理）！
  double m, n, y, z;
  m = B1 * D2 - B2 * D1;
  n = C1 * D2 - C2 * D1;
  // 不严格的比较！
  if (A * B * C == 0.0) {
    std::cout << "这两个平面平行或重合.";
  } else if (A1 == 0.0 && A2 == 0.0) {
    y = 1;
    z = -(B1 + D1) / C1;
    std::cout << "这两个平面交线的点向式方程为：" 
      << "x-(1)/" << A << '=' 
      << "y-(1)/" << B << '=' 
      << "z-(" << z << ")/" << C;
  } else if (A == 0.0) {
    y = 1;
    z = -(A1 + B1 + D1) / C1;
    std::cout << "这两个平面交线的点向式方程为：" 
      << "x-(1)/" << A << '=' 
      << "y-(" << y << ")/" << B << '=' 
      << "z-(" << z << ")/" << C;
  } else {
    y = (B + n) / A;
    z = (C - m) / A;
    std::cout << "这两个平面交线的点向式方程为：" 
      << "x-(1)/" << A << '=' 
      << "y-(" << y << ")/" << B << '=' 
      << "z-(" << z << ")/" << C;
  }
  return 0;
}
```

### 小型数据库
有点小 Bug，使用时请先在源代码所在目录下创建 Info.dat 文件。
```cpp
#include <iostream>
#include <fstream>
#include <stdlib.h>
#include <cstring>
using namespace std;
struct Info {
  int code;
  char name[20];
  int grade;
};
int choice() {
  int mychoice;
  cout << "请选择下一步操作:\n";
  cout << "0\t退出程序\n";
  cout << "1\t重新录入文件信息\n";
  cout << "2\t显示所有学生信息\n";
  cout << "3\t根据学号查找学生信息\n";
  cout << "4\t根据姓名查找学生信息\n";
  cout << "请输入:0/1/2/3/4\t";
  cin >> mychoice;
  while (mychoice < 0 || mychoice>4) {
    cout << "请选择:0/1/2/3/4\t";
    cin >> mychoice;
  }
  return mychoice;
}
void chioce_one(Info* s1, fstream* p1) {
  char again = 'y';
  while (again == 'y') {
    cout << "请输入学生信息:\n";
    cout << "学号:";
    cin >> s1->code;
    cin.ignore();
    cout << "姓名:";
    cin.getline(s1->name, 21);
    cout << "成绩:";
    cin >> s1->grade;
    cin.ignore();
    p1->write((char*)s1, sizeof(*s1));
    cout << "再输入一个学生信息吗? y/n\t";
    cin >> again;
    if (again == 'n') {
      cout << "输入完成!" << endl;
      break;
    }
    while (again != 'y' && again != 'n') {
      cout << "请选择:y/n\t";
      cin >> again;
    }
  }
}
void chioce_two(Info* s2, fstream* p2) {
  cout << "***现在显示已添加的学生信息***\n\n";
  while (!p2->eof()) {
    p2->read((char*)s2, sizeof(*s2));
    if (p2->fail()) {
      break;
    }
    cout << "学号:" << s2->code << endl;
    cout << "姓名:" << s2->name << endl;
    cout << "成绩:" << s2->grade << endl << endl;
  }
  cout << "显示完毕!" << endl;
}
void chioce_three(Info* s3, fstream* p3) {
  cout << "请输入要查找学生的学号:";
  int num;
  cin >> num;
  for (int count = 0; !p3->eof(); count++) {
    p3->seekg(count * sizeof(*s3), ios::beg);
    p3->read((char*)s3, sizeof(*s3));
    if (s3->code == num) {
      cout << "学号为" << num << "的学生信息如下:\n\n";
      cout << "学号:" << s3->code << endl;
      cout << "姓名:" << s3->name << endl;
      cout << "成绩:" << s3->grade << endl << endl;
      break;
    }
  }
  cout << "查找结束!\n";
}
void chioce_four(Info* s4, fstream* p4) {
  cout << "请输入要查找学生的姓名:";
  char who[20];
  cin >> who;
  int if_equal = 0;
  for (int count = 0; !p4->eof(); count++) {
    p4->seekg(count * sizeof(*s4), ios::beg);
    p4->read((char*)s4, sizeof(*s4));
    if_equal = strcmp(s4->name, who);
    if (!if_equal) {
      cout << "姓名为" << who << "的学生信息如下:\n\n";
      cout << "学号:" << s4->code << endl;
      cout << "姓名:" << s4->name << endl;
      cout << "成绩:" << s4->grade << endl << endl;
    }
  }//此循环有bug
  cout << "查找结束!\n";
}
int main() {
  cout << "欢迎使用学生成绩信息管理系统!\n";
  Info student;
  Info* stu = &student;
  fstream person;
  fstream* per = &person;
  int next = choice();
  while (next) {
    person.open("Info.dat", ios::in | ios::out | ios::binary);
    if (person.fail()) {
      cout << "打开文件时发生错误!" << endl;
      exit(0);
    }
    if (next == 1) {
      chioce_one(stu, per);
    }
    else if (next == 2) {
      chioce_two(stu, per);
    }
    else if (next == 3) {
      chioce_three(stu, per);
    }
    else if (next == 4) {
      chioce_four(stu, per);
    }
    person.close();
    next = choice();
  }
  cout << "程序结束!欢迎使用!" << endl;
  return 0;
}
```

### 简易计算器

```c++
#include <iostream>

namespace CalculatorCLI
{
  constexpr char NUM = 'n';
  constexpr char QUIT = 'q';
  constexpr char CALC = ';';

  struct Token
  {
    char kind = NUM;
    double value = 0.0;
  } token;  // both definition and declaration here

  Token get_token()
  {
    char kind;
    std::cin >> kind;
    switch (kind)
    {
    case QUIT:
    case CALC:
    case '+':
    case '-':
    case '*':
    case '/':
    case '(':
    case ')':
    {
      return Token{ kind, 0.0 };
    }
    case '.':
    case '0':
    case '1':
    case '2':
    case '3':
    case '4':
    case '5':
    case '6':
    case '7':
    case '8':
    case '9':
    {
      std::cin.unget();
      double value;
      std::cin >> value;
      return Token{ NUM, value };
    }
    default:
    {
      std::cout << "Bad input. Please try again.\n";
      return Token{ QUIT, 0.0 };
    }
    }  // end switch
  }

  double expression();

  double primary()
  {
    token.kind = NUM;
    for (double temp = 0.0; token.kind != QUIT;)
    {
      switch (token.kind)
      {
      case NUM:
      {
        temp = token.value;
        [[fallthrough]];
      }
      case ')':
      {
        token = get_token();
        break;
      }
      case '(':
      {
        return expression();
      }
      default:
      {
        return temp;
      }
      }  // end switch
    }
    return 0.0;
  }

  double term()
  {
    for (double temp = primary(); token.kind != QUIT;)
    {
      switch (token.kind)
      {
      case '*':
      {
        temp *= primary();
        break;
      }
      case '/':
      {
        double x = primary();
        if (fabs(x) > 1e-15)
        {
          temp /= x;
        }
        else
        {
          std::cout << "Can't be divided by zero!";
          token.kind = QUIT;
        }
        break;
      }
      default:
      {
        return temp;
      }
      }  // end switch
    }
    return 0.0;
  }

  double expression()
  {
    for (double temp = term(); token.kind != QUIT;)
    {
      switch (token.kind)
      {
      case '+':
      {
        temp += term();
        break;
      }
      case '-':
      {
        temp -= term();
        break;
      }
      default:
      {
        return temp;
      }
      }  // end switch
    }
    return 0.0;
  }

  void calculate()
  {
    std::cout
      << "CalculatorCLI v0.1 by kongolou.\n"
      << "\"That's when I read a book of Bjarne Stroustrup.\"\n\n"
      << CALC
      << " to end an expression to calculate and "
      << QUIT
      << " or any bug key to quit. :)\n\n"
      << "\n[Grammar]\n\n"
      << "Expression:\n"
      << "  Term\n"
      << "  Expression\"+\"Term\n"
      << "  Expression\"-\"Term\n"
      << "Term:\n"
      << "  Primary\n"
      << "  Term\"*\"Primary\n"
      << "  Term\"/\"Primary\n"
      << "Primary:\n"
      << "  number\n"
      << "  \"(\"Expression\")\""
      << "\n\n[Example]\n\n"
      << "> 2-(-3/5);\n"
      << "= 2.6\n"
      << "\n[Test]\n";
    for (double result = 0.0; token.kind != QUIT;)
    {
      std::cout << "\n> ";
      result = expression();
      if (token.kind == CALC)
      {
        std::cout << "= " << result;
      }
    }
  }
}

int main()
{
  CalculatorCLI::calculate();
  return 0;
}
```
