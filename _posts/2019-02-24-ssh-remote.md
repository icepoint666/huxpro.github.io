---
layout: post
title: "使用ssh进行文件传输以及远程登录图形化界面"
subtitle: 'Using ssh to transfer files and telnet GUI'
author: 'Icep'
header-img: "img/post-bg-ssh.jpg"
header-mask: 0.3
tags:
  - Linux
---

> 运行环境：远程系统是ubuntu

## 使用ssh命令行与远程服务器进行文件传输
上传本地文件到服务器
```shell
$ scp /var/www/test.php root@192.168.0.101:/var/www/
```
(把本机/var/www/目录下的test.php文件上传到192.168.0.101这台服务器上的/var/www/目录中)

从服务器上下载文件
```shell
$ scp root@192.168.0.101:/var/www/test.txt
```

从服务器下载整个目录
```shell
$ scp -r root@192.168.0.101:/var/www/test /var/www/
```
(前面为远程目录，后面为本地目录）

上传目录到服务器
```shell
$ scp -r test root@192.168.0.101:/var/www/
```

## 登录ssh后使server端的图形界面显示到client端
A端登陆到B端

首先，我们需要在B端获取A端登录的ip，也就是**获取ssh登录用户的ip地址**

step1:找sshd进程
‵‵‵shell
$ ps -ef|grep sshd
root      1693     1  0 Aug13 ?        00:00:00 /usr/sbin/sshd   #父进程号是1的是系统服务进程
root     12598  1693  2 14:59 ?        00:00:00 /usr/sbin/sshd   #有这个进程说明有SSHD远程客户登录
root     12638  1693  0 15:02 ?        00:00:00 /usr/sbin/sshd   #这是第二个登录用户
root     12633 12600  0 14:59 pts/0    00:00:00 grep sshd
```

step2:根据登录上来的进程号找到用户进程
```shell
$ ps -ef|grep 12598
root     12598  1693  0 14:59 ?        00:00:00 /usr/sbin/sshd
root     12600 12598  0 14:59 pts/0    00:00:00 -bash            #第一个用户，居然还是用root登录的。
```

step3:根据bash进程的终端号pts/?来确定来源
```shell
$ who -a|grep pts/0
root     + pts/1        Aug 14 15:02 00:03       12640 (192.168.0.123)   #登录来源IP192.168.0.123
```

**对登录端A端进行设置**
```shell
$ xhost + liang@125.216.231.242
```

修改ssh_config文件，因为A端一般都可以拥有root权限，所以可以修改将ForwardX11 no 改为 yes
```shell
$ sudo vi /etc/ssh/ssh_config
ForwardX11 yes
```

重启A端ssh
```shell
$ /etc/init.d/ssh restart
```

**对服务端B端进行设置**
设置DISPLAY环境变量
```shell
$ export DISPLAY=192.168.0.123:10.0
```
前面的192.168.0.123就是前面ssh登录端获取的ip
10代表 port_num - 6000 = 10
.0代表显示像素
如果是本地显示那么DISPLAY=:0.0

查看一下配置文件
```shell
$ cat /etc/ssh/ssh_config
```
X11Forwarding no 或者 ForwardX11Trusted yes

不然就要联系管理员获取root权限，修改配置文件

**尝试一下**
```shell
$ ssh -X liang@125.216.231.242
$ xclock
$ display
```
登录后，使用xclock或者display测试一下
