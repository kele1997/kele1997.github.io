---
layout: post
title: "ntfs交换数据流在隐写"
date: 2017-7-20 13:07:37
author: "可乐"
catalog:
tags:
  -  总结
  -  学习使我快乐

---
# NTFS交换数据流
[参考博客](http://www.cnblogs.com/Chesky/p/ALTERNATE_DATA_STREAMS.html)
[微软官方的解释](https://getpocket.com/a/read/164206703)

1. 微软的官方定义是：流是有序的字节。在NTFS文件系统中，流里面包含了数据，这些数据被写到文件中，并且给这个文件属性和内容之外的更多信息。例如：你可以创建一个包含搜索关键词的流，或者是包含文件创建者信息的流。   
（这一段是我自己翻译的233333333333)  
2.NTFS流的类型有很多，图片、文本等等
## 制作一个包括NTFS流的文件
echo "str">file:newname   
example:  echo "hello">hapi.txt:guapi.txt  
把hello这个字符串放到hapi.txt后面的流中，命名为guapi.txt   
要查看这个文件也非常简单   
`notepad hapi.txt:guapi.txt`
---
type 命令用把一个文件放到流里面  
`type 233.jpg>>hapi.txt:guapi.jpg`

3.检测这些流文件可以使用lads.exe ，把程序放到文件目录下   
`lads.exe /s`递归检测ntfs流  
'streams.exe -d' 可以用来删除ntfs流  
4.提取流文件  
[alternateStreamView](http://www.nirsoft.net/utils/alternate_data_streams.html)