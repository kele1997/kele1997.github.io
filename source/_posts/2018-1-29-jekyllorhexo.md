---
layout: post
title: "使用github静态页面和jekyll引擎搭建个人博客"
date: "2018-01-29 12:20:31"
tags:  
  - 静态页面
  - 总结
catalog: true
---
# 使用github静态页面和jekyll引擎搭建个人博客 
## 初步认识github的静态页面
[官方文档](https://help.github.com/articles/what-is-github-pages/)←点击  
大体意思就是github提供了一个静态页面功能，你不需要服务器，你可以使用jekyll主题来发布你的网站，需要注意的是**github提供的是静态页面功能，所以是不支持使用动态网站语言的，例如php,ruby,python**   
github静态页面有这么几个特点: 
1. 域名是{你的用户名}.github.io 
1. 仓库的大小显示是1GB,理论上来说是够用的  
1. 每个月的带宽流量显示是100GB，理论上来说也是够用的    

## 第一步:创建github账号，初始化仓库  
* 第一步
![2018-01-29-13-15-01-2018129](http://or4d8nhvk.bkt.clouddn.com/2018-01-29-13-15-01-2018129.png)  
如图所示，输入username，email,password进行注册,username的选定一定要**慎重**，因为username关乎你的博客域名(虽然你可以通过后期绑定域名修改),但是一定要慎重啊。:smile:

* 第二步 
![2018-01-29-13-18-10-2018129](http://or4d8nhvk.bkt.clouddn.com/2018-01-29-13-18-10-2018129.png)
创建一个新的仓库(New  repositorie),然后
在新建仓库名称的时候一定要注意吗，你的仓库名称一定要是你的{用户名}.github.io，然后你才可以通过github创建你的静态页面，然后通过http://username.github.io (这里的username用自己的代替)进行访问   
![2018-01-29-13-20-53-2018129](http://or4d8nhvk.bkt.clouddn.com/2018-01-29-13-20-53-2018129.png)   
我这里已经创建了一个，所以会报错，假如你的没有什么问题，点击最后的create repositorie绿色按钮就可以了   


## 第二步安装git
因为我们使用了github的静态页面功能，所以本质上我们网站是一个git的版本库，所以我们需要安装git工具，同时配置环境变量  
[安装](https://git-scm.com/)  
配置具体百度即可  
[git的基本使用方法](http://marklodato.github.io/visual-git-guide/index-zh-cn.html)  

## 第三步选择jekyll的主题  
* [jekyll主题网站](http://jekyllthemes.org/)←点我    
选择一个自己喜欢的主题，点击进入之后，点击demo按钮进行预览，如果喜欢的话，点击download按钮进行下载(在和demo同级的页面中),下载之后进行解压。  
* 然后同时我们把github上的仓库clone下来,在github上点击你的仓库,然后那个https开头的地址记录下来![2018-01-29-13-45-32-2018129](http://or4d8nhvk.bkt.clouddn.com/2018-01-29-13-45-32-2018129.png)

下面默认配置好环境变量  
在终端输入下面命令  
```
git clone https://github.com/kele1997/temp.git   #以你的实际地址为准
``` 
这时候你会发现你的电脑上出现了一个新的文件夹temp(以实际为准)
* 然后回到上面，把解压后的文件全部复制到你的文件夹下面，然后在cmd中输入，
```
git add *
git commit -m "注释"
git push
```
然后会提示登录github，输入账号和密码就可以了,将修改推送到github之后，等待一会，就可以了就行访问了,访问{username}.github.io，就可以看到你的网站了   

## 第四步jekyll 目录简介 
目录部分可能不一样，正常现象
```
├─css   #网站页面的css文件
├─fonts #网站的字体文件
├─img  #自己创建的，用来保存博客上的图片
│  └─in-post
│      ├─post-alitrip-pd
│      └─post-js-version
├─js  #网站里面使用的javascript文件
├─less
├─portfolio 
│  ├─css
│  ├─fonts
│  ├─images
│  └─js
├─pwa 
│  └─icons
├─_drafts # 草稿，不进行发布
├─_includes # 网页的页首和页脚 
├─_layouts # 博客使用的模板样式
└─_posts # 博客文章内容 ****
|    ├─data
|    └─img
|-_config.yml # 配置文件
```


## 第五步jekyll 配置文件的修改 
修改_config文件，通常来说config 文件有很详细的简介  
```
# Developer's Note: This is a sample _config.yml file offered with
# jekyll-theme-prologue for your convenience. To use it, move it to your
# project's root directory. Please note that the following lines are
# NECESSARY for Prologue to work correctly:
#
# theme: jekyll-theme-prologue
# collections: [sections]
#
# They activate the theme and tell Jekyll to find your homepage content
# in /_sections. Note where "sections" starts with an underscore
# and where it doesn't. The social settings will make links to
# Twitter, etc., but only if you give the URL.
#
# Also, be sure to customize url and baseurl for your site.
#
# ---------------------------------------------------------------
#
# Welcome to Jekyll!
#
# This config file is meant for settings that affect your whole blog, values
# which you are expected to set up once and rarely edit after that. If you find
# yourself editing this file very often, consider using Jekyll's data files
# feature for the data you need to update frequently.
#
# For technical reasons, this file is *NOT* reloaded automatically when you use
# 'bundle exec jekyll serve'. If you change this file, please restart the server process.

# Site settings
# These are used to personalize your new site. If you look in the HTML files,
# you will see them accessed via {{ site.title }}, {{ site.email }}, and so on.
# You can create any custom variable you would like, and they will be accessible
# in the templates via {{ site.myvariable }}.
title: Your awesome title   #网站的标题
subtitle: Your awesome subtitle # 网站的子标题
description: >- # this means to ignore newlines until "baseurl:" #个人描述
  This is the demo site for a Jekyll theme version of
  HTML5 UP's sleek, responsive site template Prologue.
author: Your Incredible Name # 个人网站的主人
email: your-email@example.com # 个人邮箱
avatar: assets/images/avatar.jpg #头像?

# You'll want to customize url and baseurl for your own site:
baseurl: "/jekyll-theme-prologue" # the subpath of your site, e.g. /blog
url: "" # the base hostname & protocol for your site

# Social settings   #各种社交账号的网址
facebook_url:
github_url:
googleplus_url:
instagram_url:
linkedin_url:
pinterest_url:
slack_url:
twitter_url:

# Google Analytics Tracking ID goes here:    # google网站分析的id
google_analytics:

# The following settings are NECESSARY for the Prologue theme to run:
theme: jekyll-theme-prologue
collections: [sections]
```

参考描述配置即可   

## 第六步使用markdown写博客  
jekyll使用markdown进行写作  
[markdown 语法中文简体版](https://www.appinn.com/markdown/)   
文章要保存成markdown格式,后缀是markdonw或者是md  
文章开头要有一个yaml头，要有特定的格式   
```
---
layout: post
title: "标题"
author: "作者"
date: "日期"
---
```
**注意保留空格**   

文章的内容使用markdown 语法进行书写即可    
之后使用Git命令进行发布   
```
git add 文件名 #尽可能使用Tab键自动补全
git commit -m "注释"
git push 
````
然后过一段时间文章就可以了访问的到了



TODO:  
- [ ] 文章置顶