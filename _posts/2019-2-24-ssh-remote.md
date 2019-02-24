---
layout: post
title: "使用ssh进行文件传输以及远程登录图形化界面"
subtitle: 'Using ssh to transfer files and telnet GUI'
author: 'Icep'
header-img: "img/post-bg-ssh-scp.jpg"
header-style: text
tags:
  - Linux
---

> 运行环境：远程系统是ubuntu

## 使用ssh命令行传输文件至远程服务器
上传本地文件到服务器
```shell
$ scp /var/www/test.php root@192.168.0.101:/var/www/
```
(把本机/var/www/目录下的test.php文件上传到192.168.0.101这台服务器上的/var/www/目录中)

从服务器上下载文件
```shell
$ scp root@192.168.0.101:/var/www/test.txt
```

从服务器下载整个目录
```shell
$ scp -r root@192.168.0.101:/var/www/test /var/www/
```
(前面为远程目录，后面为本地目录）

上传目录到服务器
```shell
$ scp -r test root@192.168.0.101:/var/www/
```
