---
layout: post
title: "Ubuntu16.04 安装nvidia-driver与cuda9.0"
subtitle: 'Install nvidia-driver and CUDA on Ubuntu16.04'
author: 'Icep'
header-img: "img/post-bg-ssh.jpg"
header-mask: 0.3
tags:
  - Linux
---

> 运行环境： Ubuntu 16.04

## 踩坑记录

官网下载 https://developer.nvidia.com/cuda-90-download-archive 下载 cuda_9.0.176_384.81_linux.run 文件

关闭 X server
```shell
$ sudo service lightdm stop
```
之后用ctrl+alt+F1 ~ F6,打开tty1 ~ 6, 登陆后运行该文件
```shell
$ sudo sh cuda_9.0.176_384.81_linux.run
```
一直按enter后选择选项

之后可能会报错：
```
The driver installation is unable to locate the kernel source
```

一般出现这种问题的原因是与CUDA官方要求的版本不符

CUDA9.0 官方支持的Ubuntu16.04的内核版本是4.4.0系列

所以我们更换内核版本

使用以下命令可以查看和安装内核版本
```shell
$ sudo apt-cache search linux|grep linux-image
```

可以直接安装4.4.0系列的一款版本
```shell
$ sudo apt-get install linux-image-4.4.0.77-generic linux-headers-4.4.0.77-generic
```

之后需要更换系统内核

### Ubuntu如何更换系统内核
首先，查询可更换的内核序号
```shell
$ cat /boot/grub/grub.cfg
```
查询已经安装的内核和内核的序号，找到文件的menuentry

仔细观察menuentry底下还有一个submenu， submenu下面包含带有缩进的menutery

以下是计数规则：

**第一个menuentry的序号是0，第二个submenu的序号是1, 以此类推，这是前面的序号，之后在1的submenu中，第一个menuentry的序号是0，第二个是1，以此类推**

记住这几个编号

修改grub更换内核
```shell
$ sudo vi /etc/default/grub
```

如果没有使用grub修改过内核，第一行是
```
GRUB_DEFAULT=0
```

0是默认kernel

我们要做的就是把GRUB_DEFAULT=0改成

```
GRUB_DEFAULT="1 >9"
```
其中第一个数字1表示前面的序列编号，因为submenu是1

其中中间的空格与大于号是不能省略的，第二个数字9表示在submenu中的序号是第9（从0开始数的）

之后更新内核，重启
```shell
$ sudo update-grub
$ sudo reboot
```

**这里更换内核为4.4.0的时候可能会导致分辨率出现问题**

查看内核是否更换
```shell
$ uname -r
```

之后再次用刚才的操作关闭图形界面去安装driver

会将`nvidia-driver`以及`nvidia-cuda-tookit`安装成功

### 将cuda相关路径加入到环境变量中

```shell
$ vi ~/.bashrc
```

添加下面的
```
export PATH=/usr/local/cuda-9.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64:$LD_LIBRARY_PATH
```

使环境变量生效
```
$ source ~/.bashrc
```

使用nvidia-smi验证安装成功

**上面这种安装完之后再把内核切换为原来的版本4.15.0系列，可能会导致nvidia-smi产生问题，所以其实是存在问题，需要安装最新的nvidia-driver的版本才行**

**而且这样的情况下会导致安装完成后，无法正常打开ubuntu图形界面**
