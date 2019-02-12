---
layout: post
title: "Linux安装sqlite3以及相关python库"
subtitle: 'How to install sqlite3'
author: 'Icep'
header-style: text
tags:
  - Linux
---

> 运行环境： Ubuntu 18.04 （不限）

下载安装包
```shell
$ wget https://www.sqlite.org/2017/sqlite-autoconf-3170000.tar.gz --no-check-certificate
```

解压，编译
```shell
$ tar zxvf sqlite-autoconf-3170000.tar.gz
$ cd sqlite-autoconf-3170000
$ ./configure --prefix=/usr/local/sqlite3 --disable-static --enable-fts5 --enable-json1 CFLAGS="-g -O2 -DSQLITE_ENABLE_FTS3=1 -DSQLITE_ENABLE_FTS4=1 -DSQLITE_ENABLE_RTREE=1"
```
安装,在下载的sqlite-autoconf-3170000文件夹里执行
```shell
$ make
$ make install
```
安装成功后 /usr/local/sqlite3会出现

验证安装是否成功（因为没有把执行文件加入环境变量，直接这样运行）

```shell
$ cd /usr/local/sqlite3/bin
$ ./sqlite3 --help
```

正常输出就说明安装成功

同样可以在/usr/lib/python3.6找到sqlite3文件夹，尝试验证是否安装成功
```python
import sqlite3
```
