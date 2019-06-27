---
layout: post
title: "如何解决python进程被kill掉后GPU显存不释放的问题"
subtitle: 'How to kill the process to release GPU memory completely'
author: 'Icep'
header-img: "img/post-bg-ssh.jpg"
header-mask: 0.3
tags:
  - Linux
---

有时候kill掉python进程，可能发现GPU显存还是没有释放

进程被kill掉后显存依旧被占

![](../img/in-post/kill-python-1.png)


试试以下几步：

1 重新开一个shell，然后输入： `ps aux|grep user_name|grep python`。

所有该用户下的python程序就会显示出来（很多在用watch命令都不会显示的进程在这里可以看到）；

2 然后再一个个用kill命令清理

![](../img/in-post/kill-python-2.png)
