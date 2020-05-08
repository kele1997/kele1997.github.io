---
layout: post
author: "kele"
date: "2018-06-02 20:34:34"
title: "sudo echo something to proc/hello permission denied"
tags:
  - linux
---
# echo something to proc/hello permission denied
在学习与proc有关的内核模块开发的时候，创建了一个可读写的proc文件，但是写的时候发生了权限错误。。。。
`sudo echo "temp" > /proc/hello`    
permission denied    

一开始看的答案以为是sudo 与suid有关啥的，结果看了另一个答案，发现原因并不是那样   
真正的原因是这样的:          
我们都知道linux中可以运行一个命令，然后把命令的输出重定向到文件中去。所以我们的命令可以这样解释`sudo echo "temp"`这是一个命令，这个命令是通过`root`权限运行的，之后把运行的结果重定向到/proc/hello中去，这时候的重定向已经变成了**普通用户**的权限。    

解决方法:   
1. su 全部都是`root`权限
2. tee 三通 echo "temp" |sudo tee /proc/hello 
3. sudo sh -c "echo "temp" > /proc/hello" #使用root权限运行sh一条命令