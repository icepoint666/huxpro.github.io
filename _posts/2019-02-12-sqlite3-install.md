---
layout: post
title: "Linux安装sqlite3以及相关python库"
subtitle: 'How to install sqlite3'
author: 'Icep'
header-style: text
tags:
  - Linux
---

> SQLite是一个进程内的库，实现了自给自足的、无服务器的、零配置的、事务性的 SQL 数据库引擎。它是一个零配置的数据库，这意味着与其他数据库一样，您不需要在系统中配置。
> 就像其他数据库，SQLite 引擎不是一个独立的进程，可以按应用程序需求进行静态或动态连接。SQLite 直接访问其存储文件。
> flask应用会使用到sqlite3, pytorch的Tensorboard也会用到sqlite3

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
