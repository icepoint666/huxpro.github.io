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

![](/img/in-post/pytorch-copy.png)

推荐裸复制方式：
```python
y = x.clone()

or 

y = x.copy()
```

**python与pytorch复制**

注意，官网中Returns a clone of a tensor. The memory is copied.内存被复制，也即重新分配了一份内存个y，所以y改变x不改变，对于copy是一样的.copy允许复制的维度不一样，但是元素个数必须是相同的。
对于Python中的深浅拷贝之类的: http://blog.csdn.net/qq_32907349/article/details/52190796

- torch中对于直接赋值的这种操作一定要小心，先看看是不是需要直接赋值，还是重新开辟一块内存来存放 
- torch中的type()函数，如果没有传入参数，表示查看Tensor的数据类型,如果传入了参数，表示将Tensor转换为想要的数据类型，数据类型由参数来指定
- torch中的Tensor所有操作例如narrow等操作，都是在原来的Tensor的基础上进行的，所以如果想要真正的复制一个Tensor就用copy和clone，如果用narrow这种操作之后经常会导致内存不连续，导致后面很多操作失效，这时候用copy和clone复制一下即可


https://blog.csdn.net/u013548568/article/details/79145401
