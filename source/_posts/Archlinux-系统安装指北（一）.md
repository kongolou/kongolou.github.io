---
title: Archlinux 系统安装指北
date: 2023-05-14 17:19:09
updated:
tags:
  - Linux
categories:
  - 沉淀
  - 指北
keywords:
  - Archlinux
  - 安装
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
## 写在前面

本文写作于 2023 年 5 月 14 日，仅为[官方教程](https://wiki.archlinuxcn.org/wiki/%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97)的拙劣注解与擅自补充。

## 第一步：安装前的准备

1. 安装介质装有安全[镜像](https://mirrors.ustc.edu.cn/archlinux/iso/2023.05.03/)
2. 周围有方便连接的稳定网络
3. 电脑禁用安全启动，调整启动顺序从安装介质启动到 Live 环境中

## 第二步：在 Live 中初步配置系统

```bash
# 1.6 验证引导模式：有显示表示是 UEFI 引导，以下以 UEFI 引导为例
$ ls /sys/firmware/efi/efivars
# 1.7 连接到互联网：无线网络通过 iwctl 验证
$ ip link
$ ping archlinux.org
^C
# 1.8 更新系统时间：联网后检查时间同步
$ timedatectl status
# 1.9 创建硬盘分区：以 /dev/sda 为例
$ fdisk -l
/dev/sda
$ fdisk /dev/sda
# 1.10 格式化硬盘分区：此处会覆盖 fdisk 里的格式化操作
$ mkfs.ext4 /dev/sda3
$ mkswap /dev/sda2
$ mkfs.fat -F 32 /dev/sda1
# 1.11 挂载分区：先挂到根分区
$ mount /dev/sda3 /mnt
$ mount --mkdir /dev/sda1 /mnt/boot
$ swapon /dev/sda2
# 2.1 更换镜像源
$ vim /etc/pacman.d/mirrorlist
# 修改，删除其他镜像源，添加以下内容
...
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
# 2.2 安装必要的软件包
$ pacstrap -K /mnt base linux linux-firmware networkmanager grub efibootmgr vim
# 3.1 生成 fstab 文件：注意用 cat 检查一下
$ genfstab -U /mnt >> /mnt/etc/fstab
$ cat /mnt/etc/fstab
```

## 第三步：进入新系统进一步配置

```bash
# 3.2 chroot 到新系统
$ arch-chroot /mnt
# 3.3 设定时区
$ ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
$ hwclock --systohc
# 3.4 本地化
$ vim /etc/locale.gen
# 修改，取消注释
...
en_US.UTF-8 UTF-8
...
zh_CN.UTF-8 UTF-8
...
$ locale-gen
$ echo LANG=en_US.UTF-8 > /etc/locale.conf
# 3.5 网络配置：设置主机名，顺便启动 NetworkManager 服务
$ vim /etc/host
# 追加
archlinux
$ systemctl enable NetworkManager
# 3.6 她说通常不需要自己创建新的 initramfs
# 3.7 设置 root 密码
$ passwd
# 3.8 安装引导程序：使用 grub
$ grub-install --efi-directory=/boot
$ grub-mkconfig -o /boot/grub/grub.cfg
# 4 退出 chroot 重启
$ exit
$ umount -R /mnt
$ reboot
```

## 欢迎来到 Archlinux 🎉🎉🎉

上述流程已经完成了 Arch 的基本部署，以下内容作为补充，仅作参考，请根据自己的喜好进行适当的选择。

### 创建日常用户

```bash
# 以 root 登录，在 wheel 小组创建一个名为 archie 的用户
$ useradd --create-home -G wheel archie
$ passwd archie
$ pacman -S sudo
$ EDITOR=vim visudo
# 修改，取消注释
...
%wheel ALL=(ALL:ALL) ALL
...
$ reboot
# 以后均以 archie 登录
```

### 配置桌面环境（KDE）

```bash
# 注意 plasma 包组中包含了 sddm
$ sudo pacman -S xorg plasma konsole
$ sudo systemctl enable sddm
$ touch /etc/sddm.conf.d
$ vim /usr/lib/sddm/sddm.conf.d/default
# 修改
...
[Theme]
Current=breeze
...
$ reboot
```

### 安装中文字体

```bash
$ sudo pacman -S noto-fonts noto-fonts-cjk noto-fonts-emoji ttf-sarasa-gothic
# 以 KDE 为例，还需进行如下设置
# 1. 打开 System Settings
# 2. 找到 Personalization/Regional Settings
# 3. 进入 Region & Language
# 4. 修改 Language
# 5. 添加 简体中文
# 6. 置顶 简体中文 并 Apply
# 7. 重启
```

### 配置中文输入法（Fcitx5）

```bash
$ sudo pacman -S fcitx5-im fcitx5-chinese-addons fcitx5-pinyin-zhwiki
$ sudo vim /etc/environment
# 追加，这里可以顺便把 vim 设置为系统编辑器
...
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
$ reboot
# 以 KDE 为例，还需进行如下设置
# 1. 打开 System Settings
# 2. 找到 Personalization/Regional Settings
# 3. 进入 Input Method
# 4. 选择 Add Input Method
# 5. 搜索 Pinyin
# 6. 点击 Add 并 Apply
```

## 挑战：一小时安装 Arch

现在你已经读完了，来完成一项挑战吧。  

```bash
$ ls /sys/firmware/efi/efivars
$ ip link
$ ping archlinux.org
^C
$ timedatectl status
$ fdisk -l
/dev/sda
$ fdisk /dev/sda
$ mkfs.ext4 /dev/sda3
$ mkswap /dev/sda2
$ mkfs.fat -F 32 /dev/sda1
$ mount /dev/sda3 /mnt
$ mount --mkdir /dev/sda1 /mnt/boot
$ swapon /dev/sda2
$ vim /etc/pacman.d/mirrorlist
...
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
$ pacstrap -K /mnt base linux linux-firmware networkmanager grub efibootmgr vim sudo
$ genfstab -U /mnt >> /mnt/etc/fstab
$ cat /mnt/etc/fstab
$ arch-chroot /mnt
$ ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
$ hwclock --systohc
$ vim /etc/locale.gen
...
en_US.UTF-8 UTF-8
...
zh_CN.UTF-8 UTF-8
...
$ locale-gen
$ echo LANG=en_US.UTF-8 >> /etc/locale.conf
$ echo archlinux > /etc/host
$ systemctl enable NetworkManager
$ passwd
$ grub-install --efi-directory=/boot
$ grub-mkconfig -o /boot/grub/grub.cfg
$ useradd --create-home -G wheel archie
$ passwd archie
$ EDITOR=vim visudo
...
%wheel ALL=(ALL:ALL) ALL
...
^D
$ umount -R /mnt
$ reboot
# 用 archie 登录
$ sudo pacman -S xorg plasma konsole
$ sudo systemctl enable sddm
$ touch /etc/sddm.conf.d
$ vim /usr/lib/sddm/sddm.conf.d/default
...
[Theme]
Current=breeze
...
$ sudo pacman -S noto-fonts noto-fonts-cjk noto-fonts-emoji ttf-sarasa-gothic
$ sudo pacman -S fcitx5-im fcitx5-chinese-addons fcitx5-pinyin-zhwiki
$ sudo vim /etc/environment
...
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
$ reboot
# 配置中文环境
```
注：在 Live 环境中，你可以通过 Alt + 左/右箭头切换控制台。开一个控制台以 root 登录，在联网条件下通过 lynx 参考 archlinux.org 的文档来作弊。

## 参考链接

- [安装指南 - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97)
- [iwd - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/Iwd#iwctl)
- [Fdisk - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/Fdisk)
- [Arch Linux 源使用帮助 — USTC Mirror Help 文档](https://mirrors.ustc.edu.cn/help/archlinux.html)
- [网络配置 - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/%E7%BD%91%E7%BB%9C%E9%85%8D%E7%BD%AE)
- [GRUB - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/GRUB)
- [建议阅读 - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/%E5%BB%BA%E8%AE%AE%E9%98%85%E8%AF%BB)
- [用户和用户组 - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/%E7%94%A8%E6%88%B7%E5%92%8C%E7%94%A8%E6%88%B7%E7%BB%84)
- [Sudo - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/Sudo)
- [Xorg - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/Xorg)
- [KDE - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/KDE)
- [SDDM - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/SDDM)
- [简体中文本地化 - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%E6%9C%AC%E5%9C%B0%E5%8C%96)
- [用 fontconfig 治理 Linux 中的字体 - 双猫CC](https://catcat.cc/post/2021-03-07/)
- [Linux 下的字体调校指南 - Leo’s Field](https://szclsya.me/zh-cn/posts/fonts/linux-config-guide/)
- [Fcitx5 - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/Fcitx5)
- [杨学峰博客 | Arch 系安装Fcitx5中文输入法](https://www.yangsihan.com/article/2022/06/29/102/)
- [Arch Linux 详细安装教程，萌新再也不怕了！「2023.04」](https://zhuanlan.zhihu.com/p/596227524)
- [The requested URL returned error: 404](https://blog.csdn.net/weixin_43833642/article/details/104335594)
