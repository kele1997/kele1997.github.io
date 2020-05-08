---
layout: post
author: "kele"
title: "easyctf writeup 学习"
date: "2018-02-23 10:13:28"
catalog: true
tags: 
  - easyctf2018 
---

# easyctf 总结学习  
比赛题目全关闭了。。。。只能根据印象来了   
[大佬的总结+题目介绍](https://github.com/mohamedaymenkarmous/CTF/tree/master/EasyCTF_IV#ezsteg)   

## Discord  
网站上有个在线聊天工具，加入进去，里面有个类似于qq的群公告，就是FLAG

## intro:Hello,world!
```
print "Hello world!"
```


## Intro:Linux 
使用网站自带的shell 登陆，flag在home目录，所以我们使用`ls`命令查看一下，发现没有什么flag,考虑是可能是隐藏文件(linux下以`.`开头的文件是隐藏文件),使用`ls -a`，发现flag文件，`cat .flag`


## The Oldest Trick in the Boo
看到公元56年，凯撒大帝   
凯撒加密 
找个在线工具解密就可以得到了


## Intro:Web
查看源码就可以得到flag   

## Soupreme Encoder  
看样子是个16进制，直接16进制转字符串    
python 16进制转字符串 
`binascii.unhexlify(str)`  

## Intro:Netcat 
使用netcat 登陆主机，然后输入你的player key，就可以得到flag 

## Hashing 
`sha512sum img.png` 就可以得到了   


## Programming: Exclusive   
```
a,b=[int(i) for i in raw_input().split(" ")]
print a^b
```

我写的比较丑，现在我参考了大佬的，超级好看的代码   
```
#python3版本
a,b=map(int,input().split(" "))
print(a^b)
```
python2 `input`需要用`raw_input`代替   

```
a,b=map(int,raw_input().split(" "))
print a^b
```

## Haystack  
这个题目找出这个`haystack`文件就可以了   

## Look At Flag   

chrome自动识别了这个文件，把他呈现成图片格式，显示出了flag,一开始我以为这个不太可能这么简单:smile:,找了半天2333

## EzSteg 
使用winhex 打开图片，搜索ascii字符`easyctf`，找到flag   
## Intro:Reverse Engineering   
holly shit :angry: 做了一下午，没搞定，看了别人的答案，结合后面的题目，学习了`xor`加密算法  http://www.ruanyifeng.com/blog/2017/05/xor.html,xor加密的特点就是在进行一次xor，就可以还原密文     
** program.py **

```
#!/usr/bin/env python3
import binascii
key = "graAhogG"
def mystery(s):
    r = ""
    for i, c in enumerate(s):
        r += chr(ord(c) ^ ((i * ord(key[i % len(key)])) % 256))
    return binascii.hexlify(bytes(r, "utf-8"))
```
solution.py 

```
#!/usr/bin/env python3
import binascii
key = "graAhogG"
def mystery(s):
    r = ""
    t=binascii.unhexlify(s).decode("utf-8")
    for i, c in enumerate(t):
        r += chr(ord(c) ^ ((i * ord(key[i % len(key)])) % 256))
    return binascii.hexlify(bytes(r, "utf-8"))
```


## Programming: Taking Input
```
a=input()
print "Hello, %s!"%a
```

## Programming: caesar cipher 
明显是个替换密码，而且只是26个英文字母的位移而已   
```
from string import maketrans
to=string.ascii_lowercase
N=int(input())
cipher=raw_input()
fro=string.ascii_lowercase[N:]+string.ascii_lowercase[0:N]
table=maketrans(fro,to)
print cipher.tranlate(table)
```

## Programming: Over and Over  
大佬写的真好看emmmm   
```
n=int(input())
print " and ".join(["over]*n)
```

## hexedit 
16进制字符串查找   

## substitute 
替换密码，[词频分析工具](https://quipqiup.com/)   
或者使用这个工具猜解除替换表https://www.guballa.de/substitution-solver    

## Markov's Bees 
flag在`/problems/markovs_bees/`这个目录下面，但是这个目录下面有好多好多的文件夹和文件，实在没有头绪，所以我们可以尝试使用grep进行按照文件内容进行搜索   

`grep -R "easyctf"`   
`-R`表示递归查找目录，就可以找到flag了

[参考资料](https://www.cnblogs.com/huninglei/p/5824205.html)


## Xor
xor加密，使用单字节进行xor加密   
** cmder ** 中的echo工具和linux中不太一样。。，所以还是使用交互式python吧   
查看文件16进制形式:
`xxd -ps xor.txt |tr -d '\n'`    
`181c0e041e091b06050a13090c0b0b120c0f070d071f131707110e1513170c0f1200`   

`binascii.hexlify("easyctf{")`    
`656173796374667b` 


选出与`656173796374667b`等长的加密信息`181c0e041e091b06`

`"".join(map(lambda x,y:chr(ord(x)^ord(y)),a,t1))`   

结果是这样的:`}}}}}}}}`  
`hex(ord(z))`
`0x7d`   
这是单字节key   
```
flag="181c0e041e091b06050a13090c0b0b120c0f070d071f131707110e1513170c0f1200"
flag_=binascii.unhexlify(flag)
result=""
for i in xrange(len(flag_)):
    result+=chr(ord(flag_[i])^ord("}"))
print result
```
## programming subset counting 
尴尬的题目  
```
a,b=[int(i) for i in raw_input().split(" ")]
#print a,b
num=[int(i) for i in raw_input().split(" ")]
#a,b=6,5
#num=[2,4,1,1,1,2]
#print num
result=0
for i in xrange(1,1<<len(num)):
    j=i
    idx=0
    temp=[]
    sum1=0
    while(j>0):
        if j&1:
            sum1+=num[idx]
            #sum1.append(num[idx])
        idx+=1
        j=j>>1
    #print sum1==b,sum1,b
    if sum1==b:
        result+=1
print result
```

## Liar 
源码是个骗子，使用shell 进行爆破数字n   
for i in {1..2000}
do
./getflag>>>$(echo $i);
done 

得到flag  


## In Plain Sight  
网站打不开，google hack得到一个youtube。。。   
看了人家的wp，发现这个flag还可以藏在dns记录里面,使用dig命令  
`dig txt blockingthesky.com`

windows:
`nslookup -qt=txt blockingthesky.com`  

## My Letter  
msoffice 文档全是压缩包，所以解压之后，就可以在文件夹下面找到flag.txt ok

## Nosource,Jr  
Damm it
又是一个xor加密    
key很明显，所以在进行一次xor加密就可以了   
https://rawsec.ml/en/EasyCTF-2018-write-ups/#80-nosource-jr-web   

## 下面不会了.... 