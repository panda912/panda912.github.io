---
title: Linux 上 Android Studio 无法连接真机
date: 2018-06-29 21:49:13
tags: Ubuntu
---

#### 现象：

![as](as.jpeg)

#### 解决：

![shell](shell.png)

`lsusb` 列出 usb 设备列表，找到对应的终端设备，记下设备 ID；

`cd /etc/udev/rules.d/` 进入该目录，参照已有的文件命名格式，创建 `.rules` 后缀文件；

输入 `SUBSYSTEM=="usb", ATTR{idVendor}=="19d2", MODE="0666", GROUP="plugdev"`, 其中 '19d2' 就是该设备的 ID。

保存 重启 👌