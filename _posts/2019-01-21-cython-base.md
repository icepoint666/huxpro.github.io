---
layout: post
title: "Cython基本用法"
subtitle: 'Using Cython in Python'
author: 'Icep'
header-img: "img/post-bg-ssh.jpg"
header-style: text
tags:
  - Python
---

## Cython基本用法

> 原文链接：https://zhuanlan.zhihu.com/p/24311879
 
### Python优缺点
**优点**

- 写起来方便
- 容易调用 C/C++ 的库

**缺点**

- 写起来方便但是以执行速度很慢为代价
- 调用C++, 需要库本身按照 Python 的规范使用 Python API

### Cython与Python对比

**例子：矩阵乘法**
```python
# dot_python.py
import numpy as np

def naive_dot(a, b):
    if a.shape[1] != b.shape[0]:
        raise ValueError('shape not matched')
    n, p, m = a.shape[0], a.shape[1], b.shape[1]
    c = np.zeros((n, m), dtype=np.float32)
    for i in xrange(n):
        for j in xrange(m):
            s = 0
            for k in xrange(p):
                s += a[i, k] * b[k, j]
            c[i, j] = s
    return c
```
Cython 程序代码：
```python
# dot_cython.pyx
import numpy as np
cimport numpy as np
cimport cython

@cython.boundscheck(False)
@cython.wraparound(False)
cdef np.ndarray[np.float32_t, ndim=2] _naive_dot(np.ndarray[np.float32_t, ndim=2] a, np.ndarray[np.float32_t, ndim=2] b):
    cdef np.ndarray[np.float32_t, ndim=2] c
    cdef int n, p, m
    cdef np.float32_t s
    if a.shape[1] != b.shape[0]:
        raise ValueError('shape not matched')
    n, p, m = a.shape[0], a.shape[1], b.shape[1]
    c = np.zeros((n, m), dtype=np.float32)
    for i in xrange(n):
        for j in xrange(m):
            s = 0
            for k in xrange(p):
                s += a[i, k] * b[k, j]
            c[i, j] = s
    return c

def naive_dot(a, b):
    return _naive_dot(a, b)

```
- Cython 程序的扩展名是 .pyx
- cimport 是 Cython 中用来引入 .pxd 文件的命令。有关 .pxd 文件，可以简单理解成 C/C++ 中用来写声明的头文件
- @cython.boundscheck(False) 和 @cython.wraparound(False) 两个修饰符用来关闭 Cython 的边界检查

Cython 的函数使用 cdef 定义，并且他可以给所有参数以及返回值指定类型。比方说，我们可以这么编写整数 min 函数：
```python
  cdef int my_min(int x, int y):
      return x if x <= y else y
```
