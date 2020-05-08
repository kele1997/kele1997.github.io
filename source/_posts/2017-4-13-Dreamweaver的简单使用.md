---
layout: post
date: 2017-4-13 20:43
title: "dreamweaver制作网页"
author: "可乐"
catalog:
tags:
  - 总结
  - 学习使我快乐
---
# dreamweaver制作asp网页
## dreamweaver的下载地址
双手奉上dreamweaver cs5的下载地址！[dreamweave下载地址](http://pan.baidu.com/s/1pL2Rp3d)，注册机自行下载（手动斜眼)。
## iis的配置问题
dreamweave最好配置好iis,否则会出现各种各样的问题（无奈脸），使用精简的asp小型服务器会出现各种各样的bug......  
1. Dreamweaver创建一个站点，然后给他添加一个服务器，否则，服务器行为不可选中。
2. iis的安装。win10控制面板的的程序与功能，启用与添加windows功能，选中internet information services,记得在万维网服务-应用程序开发功能-asp一系列功能前面打上钩√，然后安装就可以了。安装成功之后浏览`http://localhost`，这个是iis默认生成站点的一个网页。
3. 在iis中添加网站，把Dreamweaver里面新建的一个网站的路径添加进去。
4. 打开ie浏览器，在高级选项卡里面把显示友好的HTTP错误消息前面的钩去掉，然后看看asp网页打开的错误信息。
		有这么几种情况来着：
		* iis的应用程序池里面把启用32位应用程序改为True
		* 网站的asp设置，启用父路径
		* 把目录浏览启用
		* 如果打开之后默认的主页不会打开，而是显示文件目录，那就在默认文档里面添加你想作为主页的文件。
## Dreamweaver的具体操作
* 常用的元素：
		* 表格使用来布局的
		* 锚点使用来快速跳转的
		* 表单使用来提交信息的，一般采用的是post方式，搭配文本域，单选、复选使用
		* 单选框，复选框
* 数据库的操作
		* 自定义字符串"Provider=Microsoft.Jet.OLEDB.4.0;Data Source=F:\Myweb\student.mdb"
		* 绑定记录集
		* 具体的操作适合记录集里面的内容绑定
		* 服务器行为有限制登录，提交表单，更新记录，删除记录
		* 显示重复区域，可以选最近10条，也可以选择所有
		* 服务器行为有一个转到详细页面，传递一个url参数，更新记录的时候是传递一个表单参数。
# 一起哈啤
