---
layout: post
title: "Linux使用key验证方式登录ssh"
subtitle: 'Using ssh-keygen as verification to login server'
author: 'Icep'
header-img: 'img/post-bg-ssh.jpg'
header-style: text
tags:
  - Linux
---

> 运行环境： Ubuntu 18.04 （不限）

如果没有安装ssh，先要安装ssh
```shell
$ sudo apt-get install ssh
```

创建key
```shell
$ ssh-keygen -t rsa
```
需要`enter`三次
会在用户目录下生成.ssh文件夹，里面生成id_rsa(私钥），id_rsa.pub(公钥）

ps: 查看目录下.开头的文件或者文件夹命令：`ls -a`

将公钥拷贝到远程主机S上
```shell
$ scp .ssh/id_rsa.pub root@45.77.12.24:/root
```

登录到远程主机S，将刚刚传输的文件追加到authorized_keys文件中去

如果没有创建authorized_keys文件：

```shell
$ mkdir /root/.ssh
$ cd .ssh
$ vi authorized_keys
```

将公钥文件追加到/root/.ssh/authorized_keys文件末尾
```shell
$ cat ~/id_rsa.pub >> /root/.ssh/authorized_keys
```

在远程主机S中修改权限
```shell
$ chmod 700 .ssh
$ chmod 600 .ssh/authorized_keys
```
注意权限必须为700和600(或644)

回到原来的本地主机A验证
```shell
$ ssh root@45.77.12.24
Last login: Sun Dec 30 03:39:45 2018 from 125.210.206.40
```
### 禁用帐号密码验证登录
将远程Linux服务器的/etc/ssh/sshd_config配置文件中的

PasswordAuthentication 改为 no

修改后/etc/init.d/sshd reload 加载修改后的配置文件，并使其生效。

这样其他远程客户端如果没有私钥，那么这些用户连输入密码的机会都没有（注意：保存备份好主机的公钥，密钥）
