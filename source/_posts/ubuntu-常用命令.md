---
title: ubuntu 常用命令
date: 2017-02-16 22:52:38
tags: Ubuntu
---

```shell
dpkg -l | grep -i "keyword"
```

```shell
sudo apt remove --purge XXX
```

```shell
sudo apt autoremove
```

```shell
wget "http://xxxxx.zip"
```

```shell
uname -a 
```

```shell
uname -r
```

```shell
top
```

```shell
pidof xxx
```

```shell
pkill xxx
```

```shell
killall xxx
```
```shell
sudo fdisk -l
```

```shell
sudo umount /dev/sdb
```
```shell
sudo dd if=xxx.iso of=/dev/sdb
```

```shell
sudo dd if=xxx.iso of=/dev/sdb bs=2M
```

查看文件绝对路径

```shell
readlink -f file_path
```

ssh 从 server 拷贝文件到 local

```shell
scp -r username@ip_address:file_path dist_path
```

