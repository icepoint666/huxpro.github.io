---
layout: post
title: "Ubuntu18.04 配置深度学习环境"
subtitle: 'Setup Deep Learning Environment'
author: 'Icep'
header-img: "img/post-bg-ssh.jpg"
header-mask: 0.3
tags:
  - Linux
---

> 运行环境： Ubuntu 18.04

安装完系统，先更换镜像源(ustc)，然后apt-get update

### 安装显卡驱动+cuda

新版本的nvidia-driver安装同时自动可以安装对应最新版本的cuda

```shell
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt update
```
查看当前系统推荐你安装的驱动版本
```shell
ubuntu-drivers devices
```

安装最新版本
```shell
$ ubuntu-drivers devices
```

重启,查看是否安装成功，发现cuda10.1也被同时自动安装
```shell
$ nvidia-smi
Tue May  7 21:05:41 2019       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 430.09       Driver Version: 430.09       CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce RTX 2070    Off  | 00000000:01:00.0 Off |                  N/A |
|  0%   39C    P8    24W / 185W |    218MiB /  7982MiB |      4%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      1280      G   /usr/lib/xorg/Xorg                           136MiB |
|    0      1449      G   /usr/bin/gnome-shell                          80MiB |
+-----------------------------------------------------------------------------+

```
### 安装cuDNN

官网https://developer.nvidia.com/rdp/cudnn-download 下载，需要注册下账号，选对应cuda的版本的cuDNN，

下载对应的deb包：cuDNN Runtime Library for Ubuntu18.04 (Deb)

得到：libcudnn7_7.5.1.10-1+cuda10.1_amd64.deb

安装即可：
```shell
$ sudo dpkg -i libcudnn7_7.5.1.10-1+cuda10.1_amd64.deb
```

### 安装相关库

```shell
$ sudo apt-get install python3-pip
$ sudo pip3 install numpy scipy matplotlib pillow torchvision
```

