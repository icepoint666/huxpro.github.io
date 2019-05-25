---
layout: post
title: "InstanceNorm中是否加可训练的权重"
subtitle: 'If it is necessary to use trainable parameters in Instance Normalization'
author: 'Icep'
header-img: "img/post-bg-ssh.jpg"
header-mask: 0.3
tags:
  - Tricks
---

## InstanceNorm中是否加可训练的权重

**pytorch源码** 中默认InstanceNorm函数实现是建立在BatchNorm类上，但是问题是里面默认设定是`affine=False`

然而对于BatchNorm里面默认设定是`affine=True`

这个参数决定Normalization实现要不要加可训练的参数（权重）

通过了解发现，pytorch很多实现使用InstanceNorm都采取了默认设置`affine=False`,例如`pix2pixHD`,`EdgeConnect`，当然也有使用`affine=True`,例如`starGAN`

但是很多tensorflow的实现都是采取`affine=True`

**疑问：这个Instance Normalization中使用训练参数与否设定影响真的很大吗？为什么有些添加有些不加，是唯结果论吗？**
