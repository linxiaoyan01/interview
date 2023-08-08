---
title: Ubuntu16.04下编译OpenJDK12源码
categories: 
- Linux实战
tags:
- jdk
- ubuntu
---
****问题及其对应的解决方法****

**在hg.openjdk.java.net/jdk/jdk12下载zip文件极慢**
以下链接是大佬们分享的jdk12.zip源码,注意Ubuntu16.04不支持安装百度网盘,除非用deepin-wine-for-ubuntu的方式安装,我闲麻烦,直接切换到win10下安装到d盘某个目录了,然后再切换到ubuntu下的Data进行解压缩

>链接:
><https://pan.baidu.com/s/1uTTqEuxqJ1rBxccFjVwPTQ>
>提取码: juww

**无法定位软件包 openjdk-11-jdk**
读完下面这篇文章或许你就能明白为什么了

><https://zhuanlan.zhihu.com/p/55250294>

**无法获得锁 /var/lib/dpkg/lock-frontend - open (11: 资源暂时不可用) 无法获取 dpkg 前端锁 (/var/lib/dpkg/lock-frontend)，是否有其他进程正占用它？**

显然是有进程将 apt-get 占用了,那么就需要找出是哪个进程,输入 ps -aux,列出进程, 找到含有apt-get ,直接sudo kill PID解决.或者你可以强制解锁,输入命令

>sudo rm /var/cache/apt/archives/lock
>sudo rm /var/lib/dpkg/lock

**bash: configure: 没有那个文件或目录**
记住要在你准备要变编译的jdk那个目录下执行此命令
