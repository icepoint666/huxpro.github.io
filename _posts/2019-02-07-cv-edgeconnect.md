---
layout: post
title: "EdgeConnect: Generative Image Inpainting with Adversarial Edge Learning"
subtitle: '计算机视觉论文笔记————图像修复（1）'
date:       2019-02-07
author:     "Icep"
header-img: "img/post-bg-cv-edgeconnect.png"
catalog: true
tags:
  - CV论文笔记
---

> 论文链接：https://arxiv.org/pdf/1901.00212v3.pdf

### 相关工作
18年深度学习处理图像修复问题有很大进展
- 1月，Adobe组，Jiahui Yu的Deepfill v1，引入attention机制用于进行image inpainting，但是限制是mask仅限于中心正方形作为遮挡，对于测试效果还是有明显的boundary artifacts，看到很明显的生成部分与原来图像部分的分界，这方面主要原因是生成修复图片结果就是将生成图片修复部分与原来部分进行直接加和。
- 4月，NVIDIA组，提出的partial-conv，可以修复irregular holes。
- 6月，Yan et al, 提出shift-net,引入shift connection layer到U-net.
- 6月，Adobe组，Jiahui Yu的Deepfill v2，提出了门控卷积(gated-conv)解决普通卷积里把所有输入像素都视为合法输入的问题，泛化了部分卷积(partial-conv)。对于所有layers中的每个channel每个空间位置提供一种可学习的动态特征选择机制。优势是可以通过素描画笔来引导inpainting，而且适用于irregular holes.

### 文章简介

