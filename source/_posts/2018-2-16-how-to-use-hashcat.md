---
layout: post
author: "kele"
date: "2018-02-16"
title: "hashcat的使用及相关"
catalog: true
tags: 
  - hashcat
  - 工具
---
# hash的使用及其相关  
原因是我在玩[hackthissite realistic](https://www.hackthissite.org/missions/realistic/)的时候，遇到了一道题目是破解hash，看论坛上说是爆破就可以，但是我爆破了两个小时也没有效果，看了有人说是hash破解最快的方式是彩虹表，但是看看了彩虹表大多是100多个g,想想我就放弃了，最后是因为我要解密的hash是md4,我当做md5解，hashidentifier可以用来检验hash的类型  

[hashcat 官方wiki](https://hashcat.net/wiki/)   
我hashcat的版本
```
C:\Users\kele\Desktop
> hashcat64 --version
v4.0.1
```
hashcat 有四种基本的破解方式:
1. 字典破解
2. 组合字符串破解
3. 暴力破解(弃用)--mask attack(掩码破解)
4. 混合破解
还有基于规则的破解方式，还有切换大小写的不过可以归为规则破解  


## 字典破解
`-a 0 -m type hashfile dictionary1 dictionary2`    
貌似还可以用gpu加速  

## 组合字符串破解  
假如字典里面的内容是这样的 
        11
        22
        33

那么组合出来的就是: 
        1111
        1122
        1133
        2222
        2233
        3333

###  指定左侧或者右侧的字符  
         -j,  --rule-left=RULE              Single rule applied to each word on the left dictionary

        -k,  --rule-right=RULE             Single rule applied to each word on the right dictionary

举个栗子:   
字典1:

        11
        22

字典2:   

        33
        44


commands:

        -j '$-'
        -k '!$'

上面的\$代理字典里面的单词，`-j '$-'`表示在单词右侧加一个`-`，`-k '$!'`表示在单词左侧加一个`!`  
所以产生的组合是:

        11-33!
        22-33!
        11-44!
        22-44!




## Mask Attack
这个东东，官方给出来的说明比较简单，就是说mask attack 比brute-force强在减少了密码表的数量，至于减少密码数量的算法没有过多介绍(基于hcmask文件)，只是简单的提了一句通过一些常规的密码形式来减少密码数量，举个栗子："Kele1997",暴力破解的时候会枚举所有的可能，etc.. 但是mask attack的时候程序就会尝试只有首字母大写，因为大多数密码很少会在第二或者是第三的位置上有大写，通过这些规则来减少密码的候选数量     

官方说mask attack比起brute-force来说没有任何缺点，因为mask attack 可以产生所有brute-force的密码    

### Charset 字符集
    内置字符集
    ?l = abcdefghijklmnopqrstuvwxyz
    ?u = ABCDEFGHIJKLMNOPQRSTUVWXYZ
    ?d = 0123456789
    ?h = 0123456789abcdef
    ?H = 0123456789ABCDEF
    ?s = «space»!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~
    ?a = ?l?u?d?s
    ?b = 0x00 - 0xff
程序目录里面有charsets包含各种各样诡异的字符,加入你的密码中有的话，你可以使用这些文件


hashcat 有四个参数用于指定自定义的字符集
--custom-charset1=CS
--custom-charset2=CS
--custom-charset3=CS
--custom-charset4=CS
这四个参数可以用缩写 -1, -2, -3 和 -4来代替. 你可以指定自定义的字符集来进行破解

The following command sets the first custom charset (-1) to russian language specific chars:
`-1 charsets/special/Russian/ru_ISO-8859-5-special.hcchr`   


### 密码长度增量   
我们不用指定密码的固定长度，我们可以通过指定--increment 这个参数来 
```
 -i, --increment                |      | Enable mask increment mode                           |
     --increment-min            | Num  | Start mask incrementing at X                         | --increment-min=4
     --increment-max            | Num  | Stop mask incrementing at X                   
```

### Hashcat mask files 
`-a 3 hash.txt mask_file.hcmask` 加载hcmask文件，使用文件中的mask来爆破       
hashcat 自带了几个hcmask文件，放在程序目录mask/下面    

### 16进制字符
`--hex-charset` 后面的字符串是16进行  


## 混合破解
生成的密码是有字典和暴力破解生成的字符组合而成的 
举个栗子:example.dict

        password
        hello

`hashcat64 -a 6 example.dict ?d?d?d?d`   
`?d?d?d?d`表示四位长的整数   
所以最后生成的密码候选序列是：   

        password0000
        password0001
        password0002
        .
        .
        .
        password9999


`hashcat64 -a 6 ?d?d?d?d example.dict`
结果:

    0000password
    0001password
    .
    .
    .
    9999password



### 使用规则模拟混合破解 (Using rules to emulate Hybrid attack 没法子翻译了) 

使用maskprocessor利用规则产生暴力破解所需要的规则，然后生成的规则文件可以使用hashcat -r进行加载和字典混合成成密码候选
举个栗子:**example.dict**

    hello
    password


`hash -o bf.rule '$?d $?d $?d $?d`  

生成的规则是这个样子的:**bf.rule**
    
    $0 $0 $0 $0
    $0 $0 $0 $1
    $0 $0 $0 $2
    $0 $0 $0 $3
    $0 $0 $0 $4
    .
    .
    .
    $9 $9 $9 $9

然后使用`hashcat -a 6 example.dict -r bf.rule -m 。。。。`   
最后生成的是这个样子：

        hello0 0 0 0
        password0 0 0 0
        hello0 0 0 1
        password0 0 0 1
        hello0 0 0 2
        password0 0 0 2
        .
        .
        .
        hello9 9 9 9
        password9 9 9 9
    

over




