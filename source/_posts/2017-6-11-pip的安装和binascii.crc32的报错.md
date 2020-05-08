---
layout: post
author: 可乐
date: 2017-6-11 9:14
title: "pip的安装以及binascii报错问题"
tags:
  - 总结
  - 学习使我快乐
  - python
---
# pip的安装与binascii.crc32报错
## 问题起源
今天再看hbctf第一场的题解，发现一道编写python脚本爆破crc32的题目。于是尝试了一下，但是程序报错

	import binascii
	real = 0x9c4d9a5d
	for y in range(100000, 999999):
	    if real == (binascii.crc32(str(y)) & 0xffffffff):
	        print(y)
误以为是binascii模块没有安装，智障地去安装了pip，最后又重新找了问题的解决方法
##pip的安装 for windows
首先要配置要python的系统变量的PATH值。然后去[pip下载](https://pypi.python.org/pypi/pip#downloads)下载pip，解压（tar -xf **),然后打开cmd，这里的cmd要用管理员方式打开，否则安装的时候会报错，提示权限不够。   

**ps:important在这里的时候最好把x:/program(x86)/pyhon这个文件夹让当前账号获取全部权限，否则pip安装模组的时候还会报错。右键文件属性，安全，找当前账号，编辑，完全控制。**  

然后在cmd中进入解压之后的pip文件夹，在cmd输入python setup.py install ,然后pip就安装成功了

##crc32报错的真正原因
>在新版本的python3中，取消了unicode类型，代替它的是使用unicode字符的字符串类型(str),字符串类型（str）成为基础类型如下所示，而编码后的变为了字节类型(bytes)但是两个函数的使用方法不变：  
>  decode              encode   
>  bytes ------> str(unicode)------>bytes   
>  >u = '中文' #指定字符串类型对象u
str = u.encode('gb2312') #以gb2312编码对u进行编码，获得bytes类型对象str
u1 = str.decode('gb2312')#以gb2312编码对字符串str进行解码，获得字符串类型对象u1
u2 = str.decode('utf-8')#如果以utf-8的编码对str进行解码得到的结果，将无法还原原来的字符串内容   

----
[原文来源](http://www.cnblogs.com/yinsjun/p/6951588.html)
所以加一个`encode()`就可以了
