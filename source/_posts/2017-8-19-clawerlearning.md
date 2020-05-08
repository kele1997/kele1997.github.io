---
layout: post
author: kele
title: "爬虫学习笔记"
catalog: true
date: "2017-8-19"
tags:
  - 笔记
  - 学习使我快乐
---

# 用python写网络爬虫笔记   

文档(python2.7)：      
[os库的使用](http://www.cnblogs.com/MnCu8261/p/5483657.html)   
[python re正则的使用](http://www.cnblogs.com/dreamer-fish/p/5282679.html)      
[requests库](http://docs.python-requests.org/zh_CN/latest/user/quickstart.html)    
[BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html)  
[scrapy](http://scrapy-chs.readthedocs.io/zh_CN/latest/topics/images.html)   
[pickle序列化模块](http://www.cnblogs.com/cobbliu/archive/2012/09/04/2670178.html)   
[pickle官方文档](https://docs.python.org/2.7/library/pickle.html)
[datetime](https://docs.python.org/2/library/datetime.html?highlight=timedelta#datetime.timedelta)

## 第一章:
检查robots.txt 和sitemap确定网站规格    
尽可能不要去爬取那些rotbots禁止的文件夹，否则有可能被服务器封ip   
可以利用网站地图对网站进行爬取     
数百个网页的站点可以使用串行下载，如果是数百万个网页需要用到分布式  
估算网站大小可以使用google爬虫的结果 http://google.com/advanced_search site    在域名后面加上url路径可以对结果进行进一步过滤  
识别网站所用技术builtwith模块   
```
import builtwith   
builtwith.parse('http://www.baidu.com')   
```
whois协议查询域名的注册者,https://pypi.python.org/pypi/python-whois   #python-whois
Google的域名常常会阻断网络爬虫...................
1.4编写第一个网络爬虫
1.爬取
爬地图   遍历网页数据库id   追踪网页链接
换书了    

# python网络数据采集
## 第一章
这本书代码使用python3写的，对我很不友好..........   
爬取数据的时候可以尝试使用移动设备的请求头试试，可能网页变得更有规则和逻辑     
查看js文件里面的信息     
可以尝试更改爬取的数据源  
网络爬虫可以通过class属性的值分出标签来   

## 第二章
findAll(tag, attributes, recursive, text, limit, keywords)   
find(tag, attributes, recursive, text, keywords)  attributes是dict类型，多个信息用{}包裹起来example:find({"h1","h2"}）   
recursive递归参数boolean findall(text="the price") **python居然可以这么用，直接指定使用哪个参数**      
limit find等价于findall limit=1的时候  
keyword  指定属性的标签    alltext=bsobj.findall(id="text")     
tags太长时候可以使用keyword增加一个与的过滤器来简化工作      
.child 子标签   descendant 后代   
子标签是父标签的下一级，后台标签是父标签的所有下级   
next_siblings和previos_siblings函数,找到兄弟标签的最后一个标签  next_sibling 和previous_sibling 函数,返回单个标签
查找父标签parent和parents标签   
### 2.3正则表达式  
regex正则坑爹表达式
attrs返回的是一个 **字典类型**   
Lambda表达式   把一个标签作为参数，并且返回结果是布尔类型  
soup.findAll(lambda tag: len(tag.attrs)==2)  

### 2.7
可以尝试lxml 和html.parser  


## 第三章 开始采集
维基百科六度分隔理论  
使用正则匹配   
去重可以节省资源，使用python的set类型，pages=set()  
python的默认地柜是1000次，  
是时候使用scrapy采集工具了  

## 我去第四章了
json库的使用，json被转换成字典，json数组变成了列表  
scrapy库的使用  
scrapy库运行的时候调式信息太多，可以手动在setting。py中进行设置，调整调试的等级  
api调用   
列表迭代比集合快，集合查找速度更快一些  python集合就是值为none的字典，用的是hash表的结构，所以查询速度的是O(1)   
第四章有点问题googleapi没有注册成功，等注册好了，在实验一次吧   

## 第五章 存储数据
存储文件的两种方式，一种是获取url，或者是直接下载源文件  
用了别人的链接叫盗链hotlinking    
mysql基本指令  
insert into table   
create table 表名，后面是初始化的列  
drop  
delete  
python  
pymysql操作数据库小demo:  

    import pymysql
    import socket

    connection = pymysql.connect(host='localhost',
                                 user='root',
                                 password='Kele1997',
                                 db='scraping',
                                 charset='utf8mb4',
                                )
    print connection
    '''
    sql="insert into page (id,title) values (%s,%s)"
    connection.cursor().execute(sql,(5,"zzsafsfsf"))
    connection.commit()
    '''
    cur=connection.cursor()
    select2333=cur.execute('show databases;')
    print cur.fetchall()
    print cur.fetchone()
    cur.close()
    connection.close()



## 第六章
从网上直接把文件读成一个字符串，然后转换成一个 StringIO 对象，使它具有文件的
属性。

dataFile=StringIO(data)

csv类型的文件是可以进行迭代的    
csv.DictReader： 跳过csv的文件的标题

        提示这个词可能有拼写错误。
        文档的标题是由样式定义标签 <w:pStyle w:val="Title"/> 处理的。虽然不能非常简单地定
        位标题（或其他带样式的文本），但是用 BeautifulSoup 的导航功能还是可以帮助我们解决
        问题的：
        textStrings = wordObj.findAll("w:t")
        for textElem in textStrings:
         closeTag = ""
         try:
         style = textElem.parent.previousSibling.find("w:pstyle")
         if style is not None and style["w:val"] == "Title":
         print("<h1>")
         closeTag = "</h1>"
         except AttributeError:
         #不打印标签
         pass
         print(textElem.text)
         print(closeTag)
        这段代码很容易进行扩展，打印不同文本样式的标签，或者把它们标记成其他形式
# 第二部分高级数据采集

## 第七章数据清洗
n-gram  自然语言分析的时候搜索n-gram

    不过 Python 的字典是无序的，不能像数组一样直接对 n-gram 序列频率进行排序。字典内
    部元素的位置不是固定的，排序之后再次使用时还是会变化，除非你把排序过的字典里的
    值复制到其他类型中进行排序。在 Python 的 collections 库里面有一个 OrderedDict 可以
    解决这个问题：

python sort 函数
openrefine 第三方工具清洗数据u
openrefine的使用跳过了

## 第八章自然语言处理
shazam音乐雷达可以听歌识曲
哈里森总统最长的就职演说，和最短的任职时间   
使用常用的语料库来清洗数据  
使用马卡洛夫模型从中摘取文本
暂时跳过，后续补上..........看不太懂


## 第九章穿越网页表单和登录
字段名称和值，字段的值有的是经过js处理之后再传入的   
如果不确定，可以通过一些工具跟踪网页发出get和post请求的内容  
inspector chrome的审查元素  

提取文件和图像：把参数的值变成一个文件对象就ok了   
使用requests跟踪cookie，先向登录界面发送post请求，获得cookie，然后呢，使用这个cookie发送到需要登录的这个网站的其他页面
**当然还有request的另一个函数session函数**  
'''
import requests
session=requests.Session()
params={'username':'username',"password":"password"}
s=session.post("http:url",params)
print s.cookies.get_dict()
s=session.get("http:url")
print s.text
'''
*requets是一个很给力的库，可能只逊色与selenium。。。。。。。。。。*   
auth模块处理http认证 HTTPBasicAuth对象最为auth参数传入请求    
**常用的js库**    
jquery库 jquery.min.js jquery库 动态创建网页，js代码执行之后才会产生网页       
google analytics  避免被网络分析系统知道采集数据，需要确保把分析工具的cookie关掉    
google 地图    
```
var marker=new google.mps.Marker({
    position:new google.maps.LatLing(-25,2323),
    map: map
    title: "some marker text"
});
```

ajax 和动态html   
提交表单之后，网站的页面不需要重新刷新，使用了ajax技术，节省资源，避免不必要的刷新  

selenium可以打开一个浏览器，自动化的进行一系列操作，如果想要后台进行一系列操作，可以使phantomjs,  

```
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium import webdriver

driver=webdriver.PhantomJS(executable_path="C:/PentestBox/base/python/phantomjs.exe")
driver.get("http://pythonscraping.com/pages/javascript/ajaxDemo.html")

try:
    element=WebDriverWait(driver,10).until(EC.presence_of_element_located((By.ID,"loadedButton")))
finally:
    print driver.find_element_by_id("content").text
    driver.close()
```
selenium的隐式等待，还有显示等待   
隐式等待experted_conditions定义，经常使用别名EC，   
期望条件有很多   弹出提示框，一个元素被选中，页面的标题被改变了，一个元素消失了，或者出现了    
presence_of_element_located((By.ID,"loadedButton"))   
find_element   

定位器By对象    
ID html id   CLASS_NAME  CSS_SELECTOR css的id，tag,etc   
LINK_TEXT   (By.LINK_TEXT,"Next")   
PARTIAL_LINK_TEXT      通过部分链接文字来查找     
NAME   html name的属性来查找  
TAG_NAME  html标签的名称查找     
XPATH xpath表达式 beautifulsoup不支持，/div选择div节点,文档的根节点   
//div选择文档的所有的div节点   


服务器端的重定向可以使用urllib完成，客户端的重定向是用selenium    

一个比较只能的方法来检测客户端重定向是否完成，从页面加载开始就监视dom的一个元素，重复调用这个元素知道selenium抛出stableelementreferenceexception异常，说明网页已经跳转    

## 第十章图像识别与文字处理
tesseract text.tif textoutput|cat textoutput.txt   


## 第十一章避开采集陷阱
1.修改请求头,http有十几个请求头，但是只有七个最常用，
host  connection accept user-agent referrer accept-encoding accept-language
用来测试请求头的[网站](https://www.whatismybrowser.com/ ),然而人家现在付费了.....
`https://www.whatismybrowser.com/developers/what-http-headers-is-my-browser-sending`

大型网站提供了语言翻译，只需要把accept-language :修改一下，可以修改请求头成为移动设备就可以更简单的抓取了

处理cookie：
在一个网站持续地保持登录状态，需要保存一个cookie，editthiscookie chrome插件

想要出来google anaLytics的cookie，需要用到selenium和phantomjs包
```
from selenum import webdriver
driver=webdriver.PhantomJS(executable_path="")
drive.get("http://pythonscraping.com")
drive.implicitly_wait(1) 隐式等待
print driver.get_cookie()
```
还有delete_cookie() add_cookie() delet_all_cookie()方法处理


时间太快的请求可能会被封锁，所以使用多线程，增加时间间隔time.sleep(3)
控制采集速度23333333
Litmus 测试工具区分网络爬虫和使用浏览器的人类
表单的隐藏字段，用来阻止爬虫自动提交表单
一种是表单页面上的一个字段可以用服务器生成的随机变量来表示，方法采集随机变量，提交
二种是蜜罐   表单中包含一个普通名称的隐含字段，username,password，是个蜜罐圈套
避免蜜罐:css对用户不可见的元素，和隐藏表单元素不要填写


>12.4　问题检查表   
这一章介绍的大量知识，其实和这本书一样，都是在介绍如何建立一个更像人而不是更像
机器人的网络爬虫。如果你一直被网站封杀却找不到原因，那么这里有个检查列表，可以
帮你诊断一下问题出在哪里。    
• 首先，如果你从网络服务器收到的页面是空白的，缺少信息，或其遇到他不符合你预期
的情况（或者不是你在浏览器上看到的内容），有可能是因为网站创建页面的 JavaScript
执行有问题。可以看看第 10 章内容。    
• 如果你准备向网站提交表单或发出 POST 请求，记得检查一下页面的内容，看看你想提
交的每个字段是不是都已经填好，而且格式也正确。用 Chrome 浏览器的网络面板（快
捷键 F12 打开开发者控制台，然后点击“Network”即可看到）查看发送到网站的 POST
命令，确认你的每个参数都是正确的。   
• 如果你已经登录网站却不能保持登录状态，或者网站上出现了其他的“登录状态”异常，
请检查你的 cookie。确认在加载每个页面时 cookie 都被正确调用，而且你的 cookie 在
每次发起请求时都发送到了网站上。   
• 如果你在客户端遇到了 HTTP 错误，尤其是 403 禁止访问错误，这可能说明网站已经把
你的 IP 当作机器人了，不再接受你的任何请求。你要么等待你的 IP 地址从网站黑名单
里移除，要么就换个 IP 地址（可以去星巴克上网，或者看看第 14 章的内容）。如果你
确定自己并没有被封杀，那么再检查下面的内容。   
♦ 确认你的爬虫在网站上的速度不是特别快。快速采集是一种恶习，会对网管的服务
器造成沉重的负担，还会让你陷入违法境地，也是 IP 被网站列入黑名单的首要原因。
避开采集陷阱 ｜ 163    
给你的爬虫增加延迟，让它们在夜深人静的时候运行。切记：匆匆忙忙写程序或收
集数据都是拙劣项目管理的表现；应该提前做好计划，避免临阵慌乱。
♦ 还有一件必须做的事情：修改你的请求头！有些网站会封杀任何声称自己是爬虫的
访问者。如果你不确定请求头的值怎样才算合适，就用你自己浏览器的请求头吧。
♦ 确认你没有点击或访问任何人类用户通常不能点击或接入的信息（更多信息请查阅
12.3.2 节）。   
♦ 如果你用了一大堆复杂的手段才接入网站，考虑联系一下网管吧，告诉他们你的目的。
试试发邮件到 webmaster@< 域名 > 或 admin@< 域名 >，请求网管允许你使用爬虫采
集数据。管理员也是人嘛

## 13章，用爬虫测试网站
## 第十四章图像与文字的处理
待续-----


# 用python写网络爬虫笔记
## 4.3多线程爬虫   
delay标识，用来做时间间隔，防止封ip
import threading    import time
设置一个最大线程数  threading.Thread()   threading.remove(thread)   thread is still alive


setDaemon()设置守护进程，必须在start()方法调用之前设置，否则会被无限挂起，子线程启动之后，父线程也会启动
join方法用于阻塞父线程，只有子线程运行完了，才运行父线程
可以把python的内建队列改成给予mongodb的新队列，更新一个多进程     

    import multiprocessing

    def process_link_crawler(args,** kwargs)：
        num_cpus=multiprocesing.cpu_count()
        print "start      ".format(num_cpus)
        process=[]
        for i in range(num_cpus):
            p=multiprocessing.Process(target=threaded_crawler)
            args=[args],kwargs=kwargs
            p.start()
            process.append(p)
        for p in processes:
            p.join()

      


## 为链接爬虫添加缓存支持    
> linux 的文件系统是ext3/4 非法文件名是\ \0 文件名最大255字节
> osx的文件系统是hfs plus ：\0 255个utf-16编码单元
> windows NTFS  \ / ?: * "><| 255

urlparse   解析url，urlsplit componets   
字符串的开头start with 结尾end with
单纯的文件缓存系统可以使用zlib.compress来压缩一下节省空间     
存储时间戳用来判断数据是否过期需要删除
timedelta 对象设置默认过期时间为30天 pickle.dumps(),存入一个对象，用一个括号把一个时间戳和文件对象一起存进去就可以了，变成了序列化数据,带有一个属性是时间   
[timedelta](http://liuzhijun.iteye.com/blog/1874020)类型本质上来说就是一个时间差的类型，两个时间的类型可进行加减运算   

### 数据库的缓存   
>nosql数据库，这本书使用mongodb作为缓存数据库   

* nosql  
redis键值对存储，面向文档的存储mongodb，图形数据库neo4j ，列数据存储hbase    
* mongod的启动
`mongod -dbpath ./路径` 启动数据库     
```
from pymongod import MongoClient
client=MongoClient("localhost","27017")
# pymongo连接数据库
```

### threading 多线程的控制和处理
[threading]

## scrapy回调函数没有执行


## scrapy 项目在pycharm中运行


## scrapy 项目的调试