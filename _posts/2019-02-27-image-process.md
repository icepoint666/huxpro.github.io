---
layout: post
title: "图像压缩算法造成的数据改变，以及图像读写函数对比"
subtitle: 'Saving to image file will change values of image array, and talking about some image utility functions'
author: 'Icep'
header-img: "img/post-bg-ssh.jpg"
header-mask: 0.3
tags:
  - Tricks
---
## 误区：图片可以作为数组存储载体

在进行two stage或者更多stage任务的时候，我们有时候会先把第一阶段得到的结果保存成jpg/png图片，然后作为第二阶段的输入图片。

但是其实函数保存二位数组，保存为.jpg图片时，会有图像压缩算法，从而更改一些数组数值，造成信息出错。

例如我们展示一个二维数组原本第265行的数据值是：
```python
print(list(img[265,:,0]))
```
```
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
0, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 
0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```
经过下面操作
```python
img = Image.fromarray(img).convert('RGB')
img.save("temp.jpg")
img = Image.open("temp.jpg")
img = np.array(img)
print(list(img[265,:,0]))
```
发现数据发生了改变
```
[0, 0, 0, 0, 0, 15, 0, 7, 3, 0, 0, 22, 0, 3, 0, 11, 
0, 255, 245, 255, 255, 255, 246, 241, 255, 250, 255, 255, 253, 255, 242, 255, 
12, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0，0, 0, 0, 0]
```

## numpy数组的保存与读取方法
**数组以二进制格式保存**

np.save和np.load是读写磁盘数组数据的两个主要函数。默认情况下，数组以未压缩的原始二进制格式保存在扩展名为npy的文件中，以数组a为例
```python
np.save("filename.npy",a) 
b = np.load("filename.npy")
```

利用这种方法，保存文件的后缀名字一定会被置为.npy

**存取文本文件**

使用 np.savetxt 和 np.loadtxt 只能读写 1 维和 2 维的数组 

- np.savetxt：将数组写入以某种分隔符隔开的文本文件中 
- np.loadtxt：指定某种分隔符，将文本文件读入到数组中
```python
np.savetxt("filename.txt",a) 
b =  np.loadtxt("filename.txt", delimiter=',')
```

## python dict对象保存至json文件中

## 图像读写函数对比
