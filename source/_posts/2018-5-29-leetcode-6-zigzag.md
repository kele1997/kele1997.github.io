---
layout: post
author: "kele"
date: "2018-05-29 19:04:14"
title: "leetcode 6 ZigZag Conversion"
tags: 
  - leetcode
---
# 6. ZigZag Conversion 
把字符转换成一定高度的Z字型，z字形转换，主要是找规律 

```
1        9           17  
2      8 10       16 18 
3    7   11    15    19
4  6     12 14       20
5        13          21
```
找规律吧，骚年   

首先第一列的数字(1,2,3,4,5)是递增的，我们直接遍历就可以了。很容易计算出一个循环是一个竖行加一个斜行，数字的个数是2*n-2。列与列之间相隔的数字是2*n-2（n代表行数),上面隔8。对于其余的数字我们找其他的规律，看3和7吧
![201862-2018-06-02-19-59-09](201862-2018-06-02-19-59-09.png)   
3和7和下面的组成一个三角形，我利用这个三角形找规律:    
总的行数为n，我遍历的时候，当前行号为i,那么三角形的高就是n-i,三角形的两条边就差一，所以我们很容易计算7=3+(5-3)*2 ===> secondj=j+(numrows-i)*2    

代码如下:
```
class Solution {
public:
    string convert(string s, int numRows) {
        string ret="";
        int length=s.length();
        int cycle=2*numRows-2;
        for(int i=0;i<numRows;i++)
        {
          for(int j=i;j<numRows;j+=cycle)
          {
            ret+=s[j];
            int secondj=j+(numRows-i-1)*2;//i从0开始，要再减1
            if(secondj<length&&i!=0&&i!=numRows-1)//第一行和最后一行是没有斜行的
                ret+=s[secondj];
          }
        }
        return ret;
    }
};


ps:fuck一不小心string用了单引号，然后翻车了，为什么翻车呢?    
因为c语言中单引号的字符串 'a0'表示的是一个整数数字，具体内容看这里 https://blog.csdn.net/realxie/article/details/23304079   

其次就是因为string和单引号单字符相加时，不是追加字符串，而是变成了ascii值相加，因为string并没有重载和char类型的运算,所以相加的时候会发生类型转换
