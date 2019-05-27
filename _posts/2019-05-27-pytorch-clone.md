---
layout: post
title: "pytorch中的clone与copy易错的坑"
subtitle: 'Something misuse in tensor clone operation of Pytorch'
author: 'Icep'
header-img: "img/post-bg-ssh.jpg"
header-mask: 0.3
tags:
  - Pytorch
---

**pytorch中对tensor复制最好禁用裸复制，例如`y=x`**

因为这样的复制下，修改y的值同时x的值也会被修改，这种复制其实复制的是地址

![](__)

推荐裸复制方式：
```python
y = x.clone()

or 

y = x.copy()
```
