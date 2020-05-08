---
layout: post
author: "kele"
date: "2018-11-25 10:50:26"
title: "flask+apache2+wsgi+python3 部署（挖坑记）"
tags:
  - python
  - flask
  - linux
---

# flask+apache2+wsgi+python3 部署（挖坑记）  
昨天小伙伴要我帮忙写一个网页用来查询成绩，给的格式xlxs文件，我一想，思路很简单：
1. 把数据转换成数据库
2. 写个表单，传递查询学号
3. 后台用数据库查询
4. 把网站部署到服务器上


于是就有....

## xlxs 转换成数据库
本身数据就没有多少，所以我决定使用sqlite3,完全够用，有三种思路:  
1. Excel文件另存为csv文件，然后使用sqlite导入 `.import data.csv tablename`  
2. 使用openpyxl库读取Excel文件，然后逐条插入到数据库中
3. 但我google发现了一个xls2db的库，使用两行代码进行转换 `from xls2db import xls2db xls2db("data.xlxs","student.db")`

## flask 表单，路由
因为感觉比较简单，所以决定尝试一下flask，而不是用servlet。（因为java环境不想装了)

```
query_score/
        .                       
    |-- 1.xlsx              
    |-- __pycache__         
    |-- app                 
    |-- config.py           
    |-- config.pyc          
    |-- index.py            
    |-- query_score.wsgi    
    |-- requirements.txt    
    `-- student.db          
```

目录结构大致就是这个样子,主要的代码都存放在 app 文件夹下，代码暂且不表  
[参考教程](https://github.com/luhuisicnu/The-Flask-Mega-Tutorial-zh)   

flask 的运行主要在与调用 app.run(), app 是我们创建的一个 Flask 对象。所以我们可以这样运行:   
1. 在 index.py 中的写入 `from app import app` 这里的第一个app，表示的是 app目录，在Python中，目录中带有 `__init__.py` 文件夹就可以被调用，第二个app，是我们在 `__init__.py` 中创建的 app 实例 `from flask import Flask app=Flask(__name__)`，最后设置环境变量 `export FLASK_APP=index.py` ,然后运行flask run 就Ok了
2. 要么在 index.py 中写入 `if __name__=='__main__': app.run()`,然后使用python index.py进行运行


**我们需要清楚的是只要我们像运行整个网站，一定要调用 Flask 实例的 run 方法**  

这一部分真的很简单，多调试即可(况且我是新学233)


## 数据库查询 
sqlite3 数据库查询：  
1. 打开数据库
2. 执行数据库查询语句
3. 遍历查询的结果列表，处理成字典，返回（字典我用了有序字典，否则的话，最后遍历的时候，顺序是按照优化的存储顺序，字典理论上应该是hash表，所以没有顺序(推测)）


```python
import sqlite3
from collections import OrderedDict
DATABASE="./student.db"
db=sqlite3.connect(DATABASE)
cur=db.execute("select * from Sheet1 where stuno={}".format(stuno))
#获得列名
col_name=[tuple[0] for tuple in cur.decription]
for c in cur:
    #处理数据
    result=OrderedDict()
    for item,num in enumerate(c):
    result[col_name[num]]=item
    return result


```

在总结的时候我突然想起来，这种拼接sql语句的方法可能会造成 sql injection((￣▽￣)~*) 然而，我测试的时候，并没有出现，因为我在解包 cur的时候，默认认为只有一条数据（查询的时候可不是一人一条嘛）,所以处理完第一条数据，直接返回了2333,但还是推荐一下这种sql 语句格式化的方法:   

```
....
cur=db.execute("select * from Sheet1 where stuno=?",(stuno))
....
```

## 部署到服务器上
头大的一批。。。。
因为我看到大多数老哥都是用了 virtualenv来配置环境，但我头比较铁，直接使用了全局环境，最后还是怂了。。。。![-edb62886929e225](/assets/-edb62886929e225.jpg) 

大多数过程参考Pi的答案 
https://stackoverflow.com/questions/30674644/installing-mod-wsgi-for-python3-on-ubuntu/30682386#30682386   

https://stackoverflow.com/questions/30642894/getting-flask-to-use-python3-apache-mod-wsgi  




一个小技巧，偶然间想起来的 
`tail -f /var/log/apache2/error.log` 
这样会一直维持着查看apache2的错误日志   
所以我们可以开两个ssh 连接，一个修改代码，一个看日志  

大多数教程网上都有，不再赘述，只做一点经验总结：  
1. python3 和 python2 使用的 wsgi 是不一样的。因为在部署到apache2上时，使用的不是 python 解释器程序，而是使用mod_wsgi模块进行运行，所有的库都是通过它运行的，所以mod_wsgi的版本和python库的版本一定要一样
2. 有可能出现部分库无法导入的问题(importerror)，（因为有可能有两套python 环境 2.7 3.5),首先卸载2.7的库（或者全部卸载），然后重新使用python3(因为我用的是python3)的pip3进行安装 [参考](https://github.com/GeordieR/playground/issues/1)
3. 建议使用pip的时候，最好通过 `python -m pip ...` `python3 -m pip ` ，这样不会乱，因为在我的主机上，我发现pip 居然莫名其妙变成python3的了..
4. sqlite数据库打开的时候会生成一个lockfile，用来保证数据库的一致性，所以运行需要有数据库文件所在目录写权限，实在不行使用root运行apache
5. 使用virtualenv的时候那个 activate_this.py 不在python 虚拟环境下面，到[github](https://github.com/pypa/virtualenv/blob/master/virtualenv_embedded/activate_this.py)下下载，或者在virtualenv包下面找都行
6. 修改了自定义的 `/etc/apache2/sites-available/myconf.conf` 有可能因为配置文件不正确导致apache2 无法重启启动/关闭/报错，会提醒你使用`systemctl status apache2.service ` 或者 `journalctl -xe` 好看apache2 详细信息，其实可以使用 `apache2ctl configtest`检查配置文件的错误，指定定位到行，方便改正。 
7. 

