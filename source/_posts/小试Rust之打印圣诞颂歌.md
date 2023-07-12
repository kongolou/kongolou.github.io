---
title: 小试 Rust 之打印圣诞颂歌
date: 2023-03-06 20:00:52
updated:
tags:
- code
- rust
categories:
- 编程
keywords:
- rust
description: 第一次使用 Rust 编程。
top_img:
comments:
cover: https://cdn.cdnjson.com/tvax3.sinaimg.cn//large/0072Vf1pgy1foxlhjn4x1j31hc0u04fr.jpg
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
highlight_shrink: true
aside:
---

# 打印圣诞颂歌 The Twelve Days of Christmas

```rust
fn main() {
    let ordinal: [&str; 12] = [
        "first", "second", "third", "fourth", "fifth", "sixth", "seventh", "eighth", "ninth",
        "tenth", "eleventh", "twelfth",
    ];
    let lyrics: [&str; 13] = [
        "A partridge in a pear tree",
        "And a partridge in a pear tree",
        "Two turtle doves",
        "Three french hens",
        "Four calling birds",
        "Five golden rings",
        "Six geese a laying",
        "Seven swans are swimming",
        "Eight maids are milking",
        "Nine ladies dancing",
        "Ten lords are leaping",
        "Eleven pipers piping",
        "Twelve drummers drumming",
    ];
    for i in 0..12 {
        println!(
            "On the {} day of Christmas\nMy true love sent to me",
            ordinal[i]
        );
        if i == 0 {
            println!("{}", lyrics[0]);
        } else {
            let mut j = i + 1;
            while j > 0 {
                println!("{}", lyrics[j]);
                j = j - 1;
            }
        }
        println!();
    }
}
```

输出结果：

```plaintext
On the first day of Christmas
My true love sent to me
A partridge in a pear tree

On the second day of Christmas
My true love sent to me
Two turtle doves
And a partridge in a pear tree

On the third day of Christmas
My true love sent to me
Three french hens
Two turtle doves
And a partridge in a pear tree

On the fourth day of Christmas
My true love sent to me
Four calling birds
Three french hens
Two turtle doves
And a partridge in a pear tree

On the fifth day of Christmas
My true love sent to me
Five golden rings
Four calling birds
Three french hens
Two turtle doves
And a partridge in a pear tree

On the sixth day of Christmas
My true love sent to me
Six geese a laying
Five golden rings
Four calling birds
Three french hens
Two turtle doves
And a partridge in a pear tree

On the seventh day of Christmas
My true love sent to me
Seven swans are swimming
Six geese a laying
Five golden rings
Four calling birds
Three french hens
Two turtle doves
And a partridge in a pear tree

On the eighth day of Christmas
My true love sent to me
Eight maids are milking
Seven swans are swimming
Six geese a laying
Five golden rings
Four calling birds
Three french hens
Two turtle doves
And a partridge in a pear tree

On the ninth day of Christmas
My true love sent to me
Nine ladies dancing
Eight maids are milking
Seven swans are swimming
Six geese a laying
Five golden rings
Four calling birds
Three french hens
Two turtle doves
And a partridge in a pear tree

On the tenth day of Christmas
My true love sent to me
Ten lords are leaping
Nine ladies dancing
Eight maids are milking
Seven swans are swimming
Six geese a laying
Five golden rings
Four calling birds
Three french hens
Two turtle doves
And a partridge in a pear tree

On the eleventh day of Christmas
My true love sent to me
Eleven pipers piping
Ten lords are leaping
Nine ladies dancing
Eight maids are milking
Seven swans are swimming
Six geese a laying
Five golden rings
Four calling birds
Three french hens
Two turtle doves
And a partridge in a pear tree

On the twelfth day of Christmas
My true love sent to me
Twelve drummers drumming
Eleven pipers piping
Ten lords are leaping
Nine ladies dancing
Eight maids are milking
Seven swans are swimming
Six geese a laying
Five golden rings
Four calling birds
Three french hens
Two turtle doves
And a partridge in a pear tree
```

# 视频链接

[The Twelve Days of Christmas](https://www.bilibili.com/video/av54791454/?vd_source=21330d56e1fc273feed05474d179b07c)
