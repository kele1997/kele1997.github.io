---
layout: post
author: kele
date: 2017-7-25
title: "数据结构与算法--java描述"
tags:
  - 学习使我快乐
  - 总结
  - 笔记
---
# 数据结构与算法-java 描述
## 第一章 java语言的面对对象编程
* java的类型，boolean不能进行数值运算
* 整数进行32位运算，而byte是8位的，所以两个byte进行运算的时候会产生一个32位的int，直接赋值位报错
* java可以进行unicode字符赋值      
* 对象的声明不会创建对象？？？,必须new一个对象，给声明的变量赋值  
* java中类中包含了方法main(),是程序开始执行的起点，main()必须包含在一个类里面，不能是一个独立的方法
* 命令行编译 `java C.java`   运行：`java C`
* 多个类中必须有一个类中是包含main()函数的，main()的形式:`public static void main(String args[])`
* 修饰符缺省时表示包里面的任何对象都可以通过声明来访问对象的方法和域  
* 静态方法可以通过类名和对象名访问，非静态只能通过对象名来访问，记得对象声明完之后，要new创建一个对象出来
* 通用类有点像c++的模板
```class GenClass{
		object[] storge=new object[50];

