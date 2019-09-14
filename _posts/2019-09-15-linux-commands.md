---
layout: post
title: "Linux中级命令汇总"
subtitle: 'More command for linux'
author: 'Icep'
header-img: "img/post-bg-ssh.jpg"
header-mask: 0.3
tags:
  - Linux
---

**1. which语句，查看命令的可执行文件位置**
```shell
$ which nvcc
/usr/local/cuda/bin/nvcc
```

**2. 查找某个目录下符合的文件名**
```shell
$ find / -name *cuda
$ find /home/ -name *cuda*
$ find / -name *.cpp
```

**3.apt-get安装对应版本，以及查看apt-get的可安装版本**
```shell
$ sudo apt-get install package=version

示例：
$ sudo apt-get install autoconf=2.50
```
安装apt-show-versions，查看安装包的可安装版本
```shell
$ apt-get install apt-show-versions
$ apt-show-versions -a nvidia-cuda-toolkit
nvidia-cuda-toolkit:amd64 deinstall ok config-files
nvidia-cuda-toolkit:amd64 9.1.85-3ubuntu1 bionic mirrors.ustc.edu.cn
No stable version
nvidia-cuda-toolkit:amd64 not installed

```
