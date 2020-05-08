---
layout: post
date: 2017-7-23
author: "可乐"
title: "python习题集"
tags: 
  - python
  - 学习使我快乐
---
#python习题集 
[具体地址](https://github.com/Yixiaohan/show-me-the-code)   
[我的代码](https://github.com/kele1997/talk_is_cheap/tree/master/show-me-the-code)
## 第一天在图像上加红色数字
>2017-7-23

```

from PIL import Image,ImageDraw,ImageFont

def add_num(img):
	draw=ImageDraw.Draw(img)
	myfont=ImageFont.truetype('c:/windows/fonts/Arial.ttf',size=40)
	#arial.ttf是字体，size是字体大小
	fillcolor="#ff0000"
	width,height=img.size
	draw.text((width-40,0),'99',font=myfont,fill=fillcolor)
	img.save('result.jpg','jpeg')
	return 0

if __name__=='__main__':#这里的__name__和__main__是全局变量 
	image=Image.open('image.jpg')
	add_num(image)

```
坑:pentestbox中没有PIL库，`python -m pip install PIL`，提示更新pip，更新完之后，还是安装不上，`python -m pip search PIL`，确实发现有PIL库，但是却无法安装，google,提示缺少freetype库，但是pentestbox似乎不能安装，放弃。。。。   
google得:pillow是PIL的双胞胎(手动滑稽）,而且仍在更新维护，回到pentestbox，发现有pillow，于是正常使用即可.......,绕了一圈，`from PIL import Image,ImageFont,ImageDraw`    


## 第001题 生成200个激活码


	import string
	import random

	table=string.letters+string.digits#table是英文26字母大小写和数字

	s=open("temp.file","w")
	for i in xrange(200):
		s.write("".join([random.choice(table) for i in xrange(20)]))#random.choice随机选择，而且使用了list推导式，代码长度减少了
	s.close()


## 第002题 将激活码存入mysql数据库
	import pymysql
	db=pymysql.connect("localhost",user="root",password="root",db="test")
	with db.cursor() as cursor:
		sql='''create table jihuoma
		(
			id int(11) not NULL auto_increment,
			jihuo varchar(255),
			primary key('id')
		);'''

		cursor.execute(sql)
		sql2="insert into `jihuoma` ('jihuo`) values (%s)"
		file=open("temp.file")
		for line in file:
			cursor.execute(sql2,line)

		db.commit()
	
更进一步的改进是使用executemany，这个函数要比execute 和循环要快的多，还可以加入一个try except 错误处理，try抛出来的异常是一个tuple类型的，tuple[0]是错误代码，所以可以特定第针对指定的错误进行处理，在这里我要处理的是**数据库无法连接**和**数据表已经存在**两种情况


	#coding:utf-8
	import pymysql
	import sys
	try:
		db=pymysql.connect("localhost",user="root",password="root",db='test')
	except Exception as e:
		print e[1]
		sys.exit(0)
	with db.cursor() as cursor:#常见的sql命令
		sql='''create table jihuoma
		(
		id int(11) not NULL auto_increment,
		jihuo varchar(25),
		primary key (`id`)
		);'''
		try:
			cursor.execute(sql)
		except Exception as e:
			if e[0]==1050:
				print "数据表已经存在了"+e[1]
				pass
			
		sql2="insert into `jihuoma` (`jihuo`) values (%s)"
		file=open(r"D:\mycode\show-me-the-code\temp.file")
		cursor.executemany(sql2,file.readlines())
		db.commit()

## 第003题 将激活码存入redis数据库

