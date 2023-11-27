---
title: Archlinux 系统安装指北（二） Win 与 Arch 双系统
date: 2023-11-27 10:42:25
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
与 Archlinux 单系统安装相比， Windows 和 Archlinux 双系统的安装过程要复杂一些。
过程中所遇到的难以解决的问题无外乎是这两个：
- 引导问题（注意：本文只讨论 UEFI 引导）
- 时间问题

# 双系统引导（GRUB）
根据 ArchWiki 上给出的说明， Windows 和 Archlinux 双系统引导的推荐方案为：

> 先安装 Windows，再安装 Archlinux

这样做的理由是：

> Linux 在前的方案中，只有单个 Windows 分区而没有 WinRE 也没有 MSR 分区的 Windows 是否能正常工作并不明确。

## 先安装 Windows
Windows 给用户分配的 ESP 分区大小默认为 100MB ，这对多系统引导而言显然不够用。
因此安装双系统一个很重要的任务是扩展 Windows 的 ESP 分区。

由于拓展引导加载器分区的工作方式尚不明确，因此这里只推荐另外一种方法：
重新安装 Windows 系统。

下面以一台预装了 Windows 存储容量为 500G 的电脑为例，简述重新安装 Windows 的步骤：
1. 找一块空 U 盘，下载 Windows 系统安装镜像，制作系统启动引导盘（推荐 Ventoy）；
2. 备份原 Windows 系统，划出一部分容量建立非系统分区（如 D 盘 200G），将电脑上的重要文件全部塞进此分区；
3. 插上制作好的系统启动引导盘，重启系统进入 BIOS 界面，选择从 U 盘启动，运行 Windows 系统安装镜像；
4. **关键步骤：在 Windows 分区时，预先划分 500 - 1000 MB 的未分配空间（排在首位），剩余的未分配磁盘安装系统（约 300G，注意不要格式化原来 D 盘的 200G）；**
5. 安装完成后，拔下 U 盘，进入新的 Windows 系统，安装 DiskGenius 软件，将 100 MB 的 ESP 分区向前扩容 500 - 1000 MB，自此完成 ESP 分区扩容；
6. 将原 D 盘的备份文件转移到新 C 盘，删除原 D 盘，为 Archlinux 系统安装腾出空间；

完成后，磁盘布局如下：
| ESP        | MSR  | C:     | WinRE | 未分配 |
|------------|------|--------|-------|--------|
| 600-1100MB | 16MB | 约300G | 约1G  | 200G   |

## 再安装 Archlinux
与单系统相比，双系统下 Archlinux 的安装过程有所不同，主要体现在挂载分区上。
安装前的准备工作请参考前文：Archlinux 系统安装指北（一）
下面直接给出 Archlinux 系统安装过程中需要输入的命令：
```bash
# 磁盘分区与格式化（lsblk 查看，未分配磁盘应为 /dev/nvme0n1p5）
## 假设分 8G 的 SWAP（新的 /dev/nvme0n1p5）和 192G 的根目录（新的 /dev/nvme0n1p6）
$ fdisk /dev/nvme0n1p5
$ mkfs.ext4 /dev/nvme0n1p6
$ mkswap /dev/nvme0n1p5
# 挂载分区并生成配置文件
## 先挂载根目录，再挂载 ESP 分区（/dev/nvme0n1p1），生成 fstab 文件后建议查看一下
$ mount /dev/nvme0n1p6 /mnt
$ mount --mkdir /dev/nvme0n1p1 /mnt/boot
$ swapon /dev/nvme0n1p5
$ genfstab -U /mnt >> /mnt/etc/fstab
# 系统预安装
## 无线联网，修改镜像源（mirrorlist 不应该很长，如果很长的话就等一会儿再打开），安装必要组件到新系统（注意 grub 和 os-prober）
$ iwctl
$ vim /etc/pacman.d/mirrorlist
$ pacstrap -K /mnt base linux linux-firmware networkmanager grub os-prober efibootmgr vim sudo
# 用户设置
## 设置 root 密码，新建用户 archie ，赋予 sudo 权限（取消一行注释）
$ arch-chroot /mnt
$ passwd
$ useradd --create-home -G wheel archie
$ passwd archie
$ EDITOR=vim visudo
# 新系统本地化
## 配置语言（取消两行注释）、时区、主机名，这里涉及到双系统时间问题，暂不配置时区
$ vim /etc/locale.gen
$ locale-gen
$ echo LANG=en_US.UTF-8 > /etc/locale.conf
$ 
$ echo archlinux > /etc/hostname
# 安装 GRUB 引导程序
## 取消含有 GRUB_DISABLE_OS_PROBER=false 内容一行的注释
$ grub-install --efi-directory=/boot
$ vim /etc/default/grub
$ grub-mkconfig -o /boot/grub/grub.cfg
# 卸载分区并重启电脑
## 这个时候也可以选择先把桌面装上再重启，重启后记住拔掉 U 盘
$ exit
$ umount -R /mnt
$ reboot
```

现在，在 GRUB 引导界面应该能看到两个系统对应的选项了。双系统引导至此完成，完整的磁盘布局如下：
| ESP        | MSR  | C:     | WinRE | SWAP   | /    |
|------------|------|--------|-------|--------|------|
| 600-1100MB | 16MB | 约300G | 约1G  | 8G     | 192G |

# 系统时间修复
在刚才的安装步骤中，我们没有对系统时间进行处理。现在，我们来分析一下系统时间问题。

因为先安装了 Windows，所以硬件时间被 Windows 设置为了当地时间。而 Windows 和 Archlinux 默认的显示时间都是硬件时间，所以理论上无需多余配置，两个系统的时间就应该是同步的。

但唯一的问题在于，Windows 的时区在东八区，而 Archlinux 的时区在格林尼治，你居然要忍受如此丑陋的时区配置。

因此，Archwiki 给出的建议是，先将硬件时间设置为 UTC，再将各个系统的显示时间同步为本地时区时间，这样无论安装多少个系统，时间配置都是统一的。

## 修复 Windows 系统时间
> 修复时间问题的关键步骤是在 Windows 中将硬件时间设置为 UTC。

这需要通过修改注册表来完成，以管理员身份运行终端，执行如下命令
```bash
reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_DWORD /f
```
进一步在设置中，同步显示时间为本地时间。

## 修复 Archlinux 系统时间
未修复前，Archlinux 的硬件时间和显示时间都是本地时间，而时区为格林尼治。
因此修复 Archlinux 系统时间分为三步
1. 设置时区为本地时区
2. 修复显示时间为本地时间
3. 修复硬件时间为 UTC 时间

```bash
# 设置时区为本地时区
## 此命令会创建一个 /etc/localtime 软链接，指向 /usr/share/zoneinfo/ 中的时区文件
## 同 ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
$ sudo timedatectl set-timezone Asia/Shanghai
# 修复显示时间为本地时间
$ sudo timedatectl set-ntp 1
$ sudo timedatectl set-ntp 0
# 修复硬件时间为 UTC 时间
$ sudo timedatectl set-local-rtc 0
$ sudo hwclock --systohc
```

# Archlinux 系统安装遗留问题（Archinstall-problem which remains）
没错这个问题就是联网问题。

## iwctl 无线联网步骤
```bash
# 进入 iwd 环境
$ iwctl
# 首先，如果不知道你的网络设备名称，请列出所有 WiFi 设备
$ device list
# 假设你的 WiFi 设备是 wlan0，现在开始扫描网络（注意：这个命令不会输出任何内容）
$ station wlan0 scan
# 列出所有可用的网络
$ station wlan0 get-networks
# 连接到一个网络，比如是 my_wifi
$ station wlan0 connect my_wifi
# 显示包括 WiFi 设备的连接网络在内的连接状态
$ station wlan0 show
# 退出 iwd 环境
$ quit
```

## nmcli 无线联网步骤
```bash
# 前提是你已经联网安装了 NetworkManager（比如系统预安装的时候）
# 开启 Networkmanager 服务
sudo systemctl enable NetworkManager
# 显示附近的 Wi-Fi 网络
nwcli device wifi list
# 连接到 Wi-Fi 网络
nwcli device wifi connect my-wifi password 12345678
# 显示连接列表及其名称、UUID、类型和支持设备
nmcli connection show
```
