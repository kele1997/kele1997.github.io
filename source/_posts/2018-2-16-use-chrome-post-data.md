---
layout: post
author: "kele"
date: "2018-02-16 22:16:08"
title: "使用chrome post方法发送数据"
tags: 
  - chrome
  - little tricks
---
# 使用chrome post方法发送数据   
1. 使用插件postman和客户端
一开始以为还需要安装客户端，臃肿不好用，但是上手简单一试，发现还是可以，不仅仅可以用来调试网页应用程序，用来测试网站的接口，看json数据，测试爬虫也是蛮不错的    
1. 其实上面的方式类似于使用postman插件作为代理，使用postman客户端进行抓包修改,所以burpsuite没的说，肯定可以用   
1. 使用chrome 开发者工具fetch 
[超级详细的解释](https://developers.google.com/web/updates/2015/03/introduction-to-fetch)     
1. 使用xmlHttprequest() 上面的那篇文章提到过   
1. 控制台，network面板的copy as curl 
