---
layout: post
title: "Cython基本用法"
subtitle: 'Using Cython in Python'
date:       2019-01-21
author:     "Icep"
header-img: "img/post-bg-alitrip.jpg"
catalog: true
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
- 这里 np.ndarray[np.float32_t, ndim=2] 就是一个类型名就像 int 一样，只是它比较长而且信息量比较大而已。它的意思是，这是个类型为 np.float32_t 的2维 np.ndarray。
- 在函数体内部，我们一样可以使用 cdef typename varname 这样的语法来声明变量
- 在 Python 程序中，是看不到 cdef 的函数的，所以我们这里 def naive_dot(a, b) 来调用 cdef 过的 _naive_dot 函数。

### Cython程序运行流程
Cython 编译器把 Cython 代码编译成调用了 Python 源码的 C/C++ 代码把生成的代码编译成动态链接库Python 解释器载入动态链接库

要完成前两步，我们要写如下代码：
```python
# setup.py
from distutils.core import setup, Extension
from Cython.Build import cythonize
import numpy
setup(ext_modules = cythonize(Extension(
    'dot_cython',
    sources=['dot_cython.pyx'],
    language='c',
    include_dirs=[numpy.get_include()],
    library_dirs=[],
    libraries=[],
    extra_compile_args=[],
    extra_link_args=[]
)))
```
这段代码对于我们这个简单的例子来说有些太复杂了，不过实际上，再复杂也就这么复杂了，为了省得后面再贴一遍，所以索性就在这里把最复杂的列出来好了。这里顺带解释一下好了：

- 'dot_cython' 是我们要生成的动态链接库的名字
- sources 里面可以包含 .pyx 文件，以及后面如果我们要调用 C/C++ 程序的话，还可以往里面加 .c / .cpp 文件
- language 其实默认就是 c，如果要用 C++，就改成 c++ 就好了
- include_dirs 这个就是传给 gcc 的 -I 参数
- library_dirs 这个就是传给 gcc 的 -L 参数
- libraries 这个就是传给 gcc 的 -l 参数
- extra_compile_args 就是传给 gcc 的额外的编译参数，比方说你可以传一个 -std=c++11extra_link_args 就是传给 gcc 的额外的链接参数（也就是生成动态链接库的时候用的）

然后我们只需要执行下面命令就可以把 Cython 程序编译成动态链接库了。
```shell
python setup.py build_ext --inplace
```
成功运行完上面这句话，可以看到在当前目录多出来了 dot_cython.c 和 dot_cython.so。前者是生成的 C 程序，后者是编译好了的动态链接库。
