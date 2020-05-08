---
layout: post
author: "kele"
title: "metaweblog API csdn python实现"
tags: 
  - python
  - metaweblog
date: "2018-02-14 22:15:32"
---
# metaweblog API csdn python 实现 
一直想同步三个博客，梦想很大，一直在找各种工具，但是一个也没有遇到，今天突然发现了一个叫作open live writer的东西，仔细研究了一下发现是一种叫做Metaweblog API的东西，cnblog 和csdn都是支持的，到github上看了一下，发现了python实现cnblog metaweblog api的程序，所以研究了一下，写出了csdn的api 




    #coding:utf-8
    import xmlrpclib
    import os
    import sys
    import markdown2
    import httplib
    import mimetypes

    username=""
    password=""
    url="http://write.blog.csdn.net/xmlrpc/index"

    #server=xmlrpclib.ServerProxy(url)
    

    class metaWeblog_csdn:
        def __init__(self,url,username,password):
            self.url=url
            #print url,username,password
            #self.p=Proxy()
            #self.p.set_proxy("127.0.0.1:8888")
            self.user=username
            self.passwd=password
            self.server=xmlrpclib.ServerProxy(url,encoding="utf-8")
            self.blog_id=self.getUsersBlogs()[0]['blogid']
            #print self.blog_id
            self.type=["original","report","translated"]

        def newPost(self,title,description,categories,publish):
            return self.server.metaWeblog.newPost(self.blog_id,self.user,self.passwd,
                dict(title=title,description=description,categories=categories),publish)

        def getPost(self,postid):
            return self.server.metaWeblog.getPost(postid,self.user,self.passwd)

        def getCategories(self):
            return self.server.metaWeblog.getCategories(self.blog_id,self.user,self.passwd)

        def getRecentPosts(self,numberOfPosts):
            return self.server.metaWeblog.getRecentPosts(self.blog_id,self.user,self.passwd,numberOfPosts)
        
        def editPost(self,postid,title,content,categories,tags,publish,type=0,description=""):
            return self.server.metaWeblog.editPost(postid,self.user,self.passwd,
                dict(title=title,type=self.type[type],description=description,content=content,categories=",".join(categories),tags=",".join(tags)),publish)

        def newMediaObject(self,abspath):
            with open(abspath,"rb") as f:
                bits=xmlrpclib.Binary(f.read())
            type=mimetypes.guess_type(abspath)[0]
            name=os.path.basename(abspath)
            return self.server.newMediaObject(self.blog_id,self.user,dict(bits=bits,name=name,type=type))

        def getUserInfo(self):
            return self.server.blogger.getUserInfo("0123456789ABCDEF",self.user,self.passwd)

        def deletePost(self,postid,publish):
            return self.server.blogger.deletePost("0123456789ABCDEF",postid,self.user,self.passwd,publish)
        
        def getUsersBlogs(self):
            return self.server.blogger.getUsersBlogs("0123456789ABCDEF",self.user,self.passwd)

    # 只是为了通过fiddler的代理进行通信，进行抓包调试
    class Proxy(xmlrpclib.Transport):
        def set_proxy(self, proxy):
            self.proxy = proxy

        def make_connection(self, host):
            self.realhost = host
            h = httplib.HTTPConnection(self.proxy)
            return h

        def send_request(self, connection, handler, request_body):
            connection.putrequest("POST", 'http://%s%s' % (self.realhost, handler))

        def send_host(self, connection, host):
            connection.putheader('Host', self.realhost)



    
   

    if __name__=="__main__":
        if len(sys.argv)!=3:
            print "error"
        else:
            test=metaWeblog_csdn(url,username,password)
            test.newPost(sys.argv[1],markdown2.markdown_path(sys.argv[2]),[""],True)

    '''
    else:
            test=metaWeblog_csdn(url,username,password)
            with open(sys.argv[2],"r") as blog: 
                test.newPost(title,description,[""],True)
    

    if __name__=="__main__":
        test=metaWeblog_csdn(url,username,password)
        #temp=test.getCategories()[0]
        #for i in temp:
        # print i+":"+temp[i].encode("utf-8")
        #print getblogid()[0]['blogid']
    # print test.getUserInfo()
        #print os.listdir("./")
        print test.getCategories()

        #print test.getUsersBlogs()
        #print test.getRecentPosts()
        #print test.getUserInfo()
        
        #print html.encode("utf-8")
        #print test.newPost("test","test","笔记","test",True,html.encode("utf-8"),0)
        try:
            #print test.newPost("test","tests",["笔记"],["tests"],True,"test",0)
        except:
            print "erro"
    '''


比较坑的一点是发送categories的时候必须是一个列表，所以他会转化成为xmlrpclib中的array类型，然后才能正常发送，否则会一直爆500 错误   


## xmlrpclib 代理
我是通过fiddle抓包进行调试，fiddle开启的代理是127.0.0.1:8888,所以我们需要让创建severproxy的时候添加一个代理,查阅官方文档得到:
    import httplib
    class Proxy(xmlrpclib.Transport):
            def set_proxy(self, proxy):
                self.proxy = proxy

            def make_connection(self, host):
                self.realhost = host
                h = httplib.HTTPConnection(self.proxy)
                return h

            def send_request(self, connection, handler, request_body):
                connection.putrequest("POST", 'http://%s%s' % (self.realhost, handler))

            def send_host(self, connection, host):
                connection.putheader('Host', self.realhost)

`proxy=Proxy("127.0.0.1:8888")`然后构造serverproxy的时候，需要添加transport参数，`server=xmlrpclib.ServerProxy("url",transport=proxy)`
然后你就可以通过fiddle查看你发送和接受的数据包了