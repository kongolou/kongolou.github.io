---
title: SQL 学习手记
date: 2023-06-21 18:41:11
updated:
tags:
  - SQL
categories:
  - 沉淀
  - 手记
keywords:
  - SQL
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
SQL （ Structured Query Language ，结构化查询语言）是在 1974 年由 Boyee 和 Chamberlin 提出的，原名 Sequel ，并在 IBM 公司研制的关系数据库管理系统原型 System R 上实现。

1986 年 10 月，美国国家标准局批准了 SQL 作为关系数据库语言的美国标准。 1987 年，国际标准化组织也通过了这一标准，并至今得以不断发展和完善。虽然制定了标准，不同的厂商也有不同的风格，各自的 SQL 会有一些小差异。

SQL 的特点包括：
- 综合统一
- 高度非过程化
- 面向集合的操作方式
- 以同一种语法结构提供多种使用方式
- 语言简洁，易学易用

SQL 的三级模式示意图：

```plaintext
              (SQL)
                |
    +-----------+------------+
 (View1)        |         (View2)
    |         +-+            |
    +         +         +----+----+
(Table1)  (Table2)  (Table3)  (Table4)
    |         |         |         |
    +---------+---------+         +
           (File1)             (File2)
```

# 数据类型

| 字符类型 ||
| --- | --- |
| char(n) | 定长字符串 |
| varchar(n) | 变长字符串 |
| clob | 字符串大对象 |
| blob | 二进制大对象 |

| 数字类型 ||
| --- | --- |
| int | 整型， 4 bytes |
| smallint | 短整型， 2 bytes |
| bigint | 大整型， 8 bytes |
| dec(p,d) | p 位浮点数，带 d 位小数 |
| real | 单精度浮点数 |
| doubleprecision | 双精度浮点数 |
| float(n) | n 位浮点数 |

| 约定类型 ||
| --- | --- |
| boolean | 逻辑布尔型 |
| date | 日期型，格式 YYYY-MM-DD |
| time | 时间型，格式 HH:MM:SS |
| timestamp | 时间戳型 |
| interval | 时间间隔型 |

# 数据查询

## 单表查询

```sql
select [all | distinct] <column_name>
from <table_name> | <view_name>                   
[where <condition> |
 group by <column_name> having <condition> |
 order by <column_name> [asc | desc]];
```

- distinct 选项用于过滤重复行
- desc 选项用于查询结果降序排列

| 逻辑符 || 备注 |
| --- | --- | --- |
| 比较符 | = , > , < , != , >= , <= | <> , !< , !> 同后三项 |
| 与或非 | and , or , not | and 优先级高于 or ， not 可以非掉本表所有选项 |
| 闭区间 | between … and … | 等价于 x >= a and x <= b |
| 空值 | is null | 否定为 is not null |
| 包含 | in | ... in ('CS', 'MA', 'IS'); |
| 匹配 | like | 通配符：_ 表单字符，% 表字符串 |

| 聚类函数 ||
| --- | --- |
| count() | 统计元组个数 |
| sum() | 对组求和 |
| avg() | 对组求平均值 |
| max() | 对组求最大值 |
| min() | 对组求最小值 |

### 三值逻辑表

| x | y | x and y | x or y |
|---|---|---------|--------|
| T | T | T       | T      |
| T | U | U       | T      |
| T | F | F       | T      |
| U | T | U       | T      |
| U | U | U       | U      |
| U | F | F       | U      |
| F | T | F       | T      |
| F | U | F       | U      |
| F | F | F       | F      |

- 空值与任意值的算术运算结果为空值
- 空值与任意值的比较运算结果为 unknown

## 连接查询

- 等值连接：需要等号，否则为非等值连接
- 自身连接：需要为自己取两个别名
- 多表连接：需要用逻辑连接词连接

## 嵌套查询

| 子查询 ||
| --- | --- |
| any( ) | 存在量词 |
| all( ) | 全称量词 |
| exists( ) | 判断是否存在 |

|集合查询||
| --- | --- |
| union | 并 |
| intersect | 交 |
| except | 差 |

**派生查询**：所查询的基本表来自查询 

# 数据定义

## 创建 CREATE

### 创建模式

```sql
create schema <schema_name> 
authorization <author_name>;
```

- 模式名默认与作者同名
- 首先定义模式

### 创建表

```sql
create table "schema_name".table_name (
  <column_name1> <type1> [<列级约束>],
  [<column_name2> <type2> [<列级约束>], ...]
  [<表级约束>]
);
```
#### 关系完整性约束

- 列级约束 `primary key`
- 表级约束 `primary key (<column_name1>, ...)`

#### 参照完整性约束

- 表级约束 `foreign key (<column_name>) references <table_name> (<column_name>)`

#### 用户自定义完整性约束

- 唯一值 `unique`
- 非空值 `not null`
- 满足其他条件 `check <condition>`

### 创建索引

```sql
create [unique | cluster] index <index_name> 
on <table_name> (<column_name> [asc | desc]);
```

- cluster 选项创建聚簇(cu)索引

### 创建视图

```sql
create view <view_name> (<column_name>)
as <子查询>
[with check option];
```

- 子查询可以是任意的 select 语句
- with check option 控制视图权限

## 销毁 DROP

- 销毁模式 `drop schema <schema_name> restrict | cascade;`
- 销毁表 `drop table <table_name> [restrict | cascade];`
- 销毁索引 `drop index <index_name>;`
- 销毁视图 `drop view <view_name> [cascade];`

注意：销毁表自动销毁索引

## 修订 ALTER

### 修订表

```sql
alter table <table_name>
add | drop | alter 
column <column_name> <type> <列级约束> | 
constraint <constraint_name> <完整性约束>
[restrict | cascade];
```

### 修订索引

```sql
alter index <old_index>
rename to <new_index>;
```

注意：无法修订模式和视图

## 数据操纵

对表和视图

- 插入 `insect into <table_name> (<column_names>) values (<值> | <子查询>);`
- 更新 `update <table_name> set <expression> where <condition>;`
- 删除 `delete from <table_name> where <condition>;`

## 数据控制

- 授权 `grant <权限> on <对象类型> <对象名> to <用户名>`
- 夺权 `revoke <权限> on <对象类型> <对象名> from <用户名>`
