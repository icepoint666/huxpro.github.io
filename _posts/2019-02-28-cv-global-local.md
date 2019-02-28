---
layout: post
title: "Globally and Locally Consistent Image Completion"
subtitle: '计算机视觉论文笔记————图像修复（2）'
date:       2019-02-28
author:     "Icep"
header-img: "img/post-bg-cv-global-local.png"
header-mask: 0.3
catalog: true
mathjax: true
tags:
  - CV论文笔记
---

> 论文链接：http://iizuka.cs.tsukuba.ac.jp/projects/completion/data/completion_sig2017.pdf

### 文章简介
本文提出的图像补全算法，可以保持图像在全局与局部都一致。mask可以是任意形状。

### 网络结构
主要是用GAN的思路，总共有生成器与鉴别器两个部分三个网络。

生成部分：生成网络

鉴别部分：全局鉴别器+局部鉴别器

网络中激活层：Relu，最后一层采用sigmoid函数，使输出范围在0-1之间

![img](/img/in-post/post-cv-2019/global-local-1.png)

输入是RGB图像+mask，输出是RGB图像

生成网络：作者采用12层卷积网络对预处理的原始图片进行encoding，下采样两次，得到一张原图大小1/4 x 1/4大小的网格。然后再对该网格采用4层卷积网络进行decoding，从而得到复原图像。

![img](/img/in-post/post-cv-2019/global-local-2.png)

鉴别网络：一个全局鉴别器(Global Discriminator)以及一个局部鉴别器(Local Discriminator)。全局鉴别器将完整图像作为输入，识别场景的全局一致性，而局部鉴别器仅在以填充区域为中心的原图像4分之一大小区域上观测，识别局部一致性。通过采用两个不同的鉴别器，使得最终网络不但可以使全局观测一致，并且能够优化其细节，最终产生更好的图片填充效果.

![img](/img/in-post/post-cv-2019/global-local-3.png)

全局鉴别网络输入是256X256，RGB三通道图片，局部网络输入是128X128，RGB三通道图片，根据论文当中的设置，全局网络和局部网络都会通过5X5的convolution layer，以及2X2的stride降低分辨率，最终分别得到1024维向量。然后，作者将全局和局部两个鉴别器输出连接成一个2048维向量，通过一个全连接然后用sigmoid函数得到整体图像一致性打分

### 损失函数



