---
title: Linux C++开发环境搭建
toc: true
date: 2018-07-10 15:39:36
---



# 缘由


一直想知道，做  linux 上 C++ 开发的，都是怎么搭建开发环境的？应该是用的samba，映射到本地，然后开发的，这样的话，linux 机用虚拟机还是用服务器？而且，映射的时候，git要怎么做？

git 是直接从 代码库里clone 到linux上吗？然后push的时候登陆到 linux上进行 push 吗？

还是说git把代码 clone到windows的 samba的共享文件夹里面？然后使用的windows的git进行push？

而且编译的时候是使用的VS进行编译吗？还是说只是在VS里面进行编辑，然后在linux上使用makefile 和gdb进行编译和调试？嗯 感觉应该是在linux上使用makefile和gdb进行编译和调试的。








# REF

1. [快速配置 Samba 将 Linux 目录映射为 Windows 驱动器，用于跨平台编程](http://zyan.cc/samba_linux_windows/)
2. [炼丹工具集-PyCharm 利用SFTP远程炼丹](https://zhuanlan.zhihu.com/p/37361332) 他这里说比 samba 还好用
