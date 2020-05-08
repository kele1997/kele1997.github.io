---
layout: post
author: 可乐
date: 2017-11-26
title: "特殊的矩阵格式转化成为图片"
tags:
  - python
  - 总结
---
# 特殊的矩阵格式转化为图片
## 产生矩阵
![](http://or4d8nhvk.bkt.clouddn.com/17-11-26/65768272.jpg)  
[附件下载](./data/pic.bin)    


    import struct
    with open(r"pic.bin","wb") as file: #打开文件的模式必须是b,否则会在读取过程中EOF提前出现
        for i in xrange(3):
            print struct.unpack("I",file.read(4))#file read 每次读入4个字节，一个字节有8位， 也就是32位，然后使用struct模块将32数据变成一个32整数，打印出矩阵的信息来

        row=253
        col=380
        feiling=92760

        list=[[0]*col for i in xrange(row)]
        for i in xrange(feiling):
            row1=struct.unpack("I",file.read(4))
            line1=struct.unpack("I",file.read(4))
            num=struct.unpack("d",file.read(8))
            list[row1[0]-1][line1[0]]=num[0]


这个样子虽然能够产生矩阵，但是对于题目意义不大，因为这个矩阵是list二维数组，后期没用过，我直接使用PIL库转化成图片了...,我觉得题目的要求应该是使用numpy和matplotlib包进行绘制图形

    import numpy
    M=numpy.zeros((row,col))
    for i in xrange(feiling):
        row1=struct.unpack("I",file.read(4))[0]
        line1=struct.unpack("I",file.read(4))[0]
        num=struct.unpack("d",file.read(8))[0]
        M[row-1][line1-1]=num


## 转化成为图片

    #coding:utf-8
    import struct
    from PIL import Image
    with open(r"C:\Users\Kele\Desktop\pic.bin","rb") as file:
        for i in xrange(3):
            print struct.unpack("I",file.read(4))
        row=253
        col=380
        feiling=92760
        img=Image.new("L",(col,row),"black")

        for i in xrange(feiling):
            row1=struct.unpack("I",file.read(4))
            line1=struct.unpack("I",file.read(4))
            num=struct.unpack("d",file.read(8))
            img.putpixel((line1[0]-1,row1[0]-1),int(num[0]))

        img.show()

直接转化成为图片了，一开始使用"white"作为底色，图形少了一半，改成黑色，图片整个都显示了



** 逼死强迫症系列 **
    #coding:utf-8
    import struct
    import numpy
    import matplotlib.pyplot as plt


    file=open(r"C:\Users\Kele\Desktop\pic.bin","rb")

    file.seek(12,0)
    row=253
    col=380
    feiling=92760
    M=numpy.zeros((row,col))
    for i in xrange(feiling):
        row1=struct.unpack("I",file.read(4))[0]
        col1=struct.unpack("I",file.read(4))[0]
        num=struct.unpack("d",file.read(8))[0]
        M[row1-1][col1-1]=num

    print M
    plt.subplot(111)
    plt.imshow(M,cmap='gray')
    plt.show()

实在受不了了，换了正规做法
