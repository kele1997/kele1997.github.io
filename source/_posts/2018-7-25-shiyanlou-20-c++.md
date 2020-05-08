---
layout: post
author: "kele"
date: "2018-07-25 21:48:21"
title: "实验楼 第 20 期 c++ 再深入"
tags: 
  - c++
---
# 实验楼 第 20 期 c++ 再深入 
今天完成了实验楼第 20 期的三道题目 pretty interesting     

## 优化，优化!  
优化代码性能，这个题目很简单，首先想到的就是去掉递归，最容易想到的，就是类似于动态规划中备忘录的方法  

```
int fib(const int i){
    int tmp[46];
    tmp[0]=0;
    tmp[1]=1;
    for(int i=2;i<=45;i++)
        tmp[i]=tmp[i-1]+tmp[i-2]
    return tmp[45];
}
```
居然没通过，可能时因为开内存太耗费时间了，于是百度了下C++的优化tricks，但是无果,所以还是决定从算法上优化    

```
int fib(const int i){
    int a,b,temp;
    a=0;
    b=1;
    for(int i=2;i<=45;i++)
    {
        temp=b+a;
        a=b;
        b=temp;
    }
    return temp;
}
```
通过了    

## 超越打印  
简单介绍一下c++ 模板参数列表    
### c++ 模板 参数列表 
c++ 11 中引入了一个元运算符`...` (ps:在c++ prime plus上看到的) 表示多个参数，所以就有这种模板函数中的参数列表      
```
template<typename...Args>//Args这是模板参数包
void show_var(Args... args)//args是函数参数包
{

}
```   
要把参数包中的参数解包，一个一个的处理，所以可以使用递归    
```
template<typename...Args>//Args这是模板参数包
void show_var(Args... args)//args是函数参数包
{
    show_var(args);//递归死循环，因为参数数量不会减少
}
```   
**改进**   
参数包传入的时候，会解包处一个作为第一个参数value，剩余的作为args    

```
//参数为空时退出递归


void show_var(){}//当最后参数全部解包完成之后，参数为空，调用这个函数，退出递归函数

template<typename T,typename... Args>
void show_var(T value,Args... args)
{
    cout<<value<<endl;
    show_var(args...)://args表示剩余的参数
}
```

```
//参数为一个时特殊处理，退出递归
template<typename T>
void show_var(T value)
{
    cout<<"this is the last "<<value<<endl;
}

template<typename T,typename... Args>
void show_var(T value,Args... args)
{
    cout<<value<<endl;
    show_var(args...)://args表示剩余的参数
}
```   

简单说一下std:move的作用吧，移动，右值引用，就是引用临时变量   
     
最后的代码:    
```
#include <iostream>
#include <string>
#include <chrono>

struct Shiyanlou {
    std::string description;
};

// TODO START
template<typename T>
auto shiyanlou_printf(T &value)
{std::cout<<value<<std::endl;}
template<class Shiyanlou>
auto shiyanlou_printf(Shiyanlou a)
{std::cout<<a.description<<std::endl}

template<typename T, typename... Args>
auto shiyanlou_printf(T &value, Args... &args) {
	shiyanlou_printf(value);
	shiyanlou_printf(args...);




}
// TODO END

int main() {
    Shiyanlou shiyanlou {"Learn by Doing"};

    auto t1 = std::chrono::high_resolution_clock::now();

    shiyanlou_printf(1, 2, 3.14, 5, 6, 7, 8, 9, 10, 11, 12, 13, "Shiyanlou C++ Contest", std::move(shiyanlou));

    auto t2 = std::chrono::high_resolution_clock::now();
    auto duration = std::chrono::duration_cast<std::chrono::microseconds>(t2 - t1).count();
    std::cout << duration << std::endl;

    return 0;
}
```


## 无序的日志 
对输出流加锁     
对输出流加锁，防止两个线程同时向输出流中输出     

这个题目没怎么看懂，最后发现没看懂的人还有很多。。。。，答案很严肃地对map_lock 加锁、解锁，然后就通过了，坑爹啊，完全没有和ostream挂钩。。。。。。     
正确的思路应该是这样的：      
题目中有两个全局变量 map_lock stream_locks   
```
static std::mutex map_lock;
static std::map<std::ostream *, std::unique_ptr<std::mutex>> stream_locks;
```     
其中map_lock是用来对下面stream_locks上锁、解锁的，用来限制线程对于stream_locks的访问；而stream_locks才是用来对ostream上锁、解锁的，所以正确的做法是:    

```
// shiyanlou_lock.cpp
#include <iostream>
#include <chrono>
#include <mutex>
#include <thread>
#include <ostream>
#include <map>

static std::mutex map_lock;
static std::map<std::ostream *, std::unique_ptr<std::mutex>> stream_locks;

// TODO START
std::ostream& shiyanlou_lock(std::ostream& os) {
	map_lock.lock();
	auto iter = stream_locks.find(&os);
	if( iter != stream_locks.end()){//stream_locks中已经与这个ostream了
        iter->second->lock();
		map_lock.unlock();
		return os;
	}
	stream_locks.insert(std::make_pair(&os, std::unique_ptr<std::mutex>(new std::mutex)));
	iter = stream_locks.find(&os);
	iter->second->lock();
	map_lock.unlock();
	return os;


}

std::ostream& shiyanlou_unlock(std::ostream& os) {
	map_lock.lock();
	auto iter = stream_locks.find(&os);
	iter->second->unlock();   
     map_lock.unlock();
    return os;   


}
// TODO END

void locked_outputs(size_t i) {
    std::string str {"Shiyanlou - Contest: Learn by Doing - Log System"};
    std::cout << shiyanlou_lock << "Thread " << i << " | 1.1.1.1 - [2018/05/08 00:00:01] \"GET /assets/logo.png HTTP/1.1\" 200" << std::endl << shiyanlou_unlock;
    std::this_thread::sleep_for(std::chrono::seconds(1));
    std::cout << shiyanlou_lock << "Thread " << i << " | 1.1.1.2 - [2018/05/08 00:00:02] \"GET /assets/logo.png HTTP/1.1\" 200" << std::endl << shiyanlou_unlock;
    std::this_thread::sleep_for(std::chrono::seconds(1));
    std::cout << shiyanlou_lock << "Thread " << i << " | 1.1.1.3 - [2018/05/08 00:00:03] \"GET /assets/logo.png HTTP/1.1\" 200" << std::endl << shiyanlou_unlock;
    std::cout << shiyanlou_lock << "Thread " << i << "---------------------" << std::endl << shiyanlou_unlock;
}

int main() {
    std::cout << "Locked output stream" << std::endl;
    std::thread thread1(locked_outputs, 1);
    std::thread thread2(locked_outputs, 2);
    thread1.join();
    thread2.join();
}
```