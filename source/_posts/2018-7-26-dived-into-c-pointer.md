---
layout: post
author: "kele"
date: "2018-07-26 18:09:23"
title: "深入理解 c 指针学习"
catalog: true
tags: 
  - c语言
---
# 深入理解 c 指针学习
看《深入理解c指针》复习了一些c指针的东西     

## 常量指针
### 指针的读法   
指针的读法应该是从右向左读，举几个例子     
`int * const cpi=&num;`     
1. cpi是一个变量                ` cpi`
2. cpi是一个常量                 `const cpi`
3. cpi是一个常量指针            `  * const cpi`
4. cpi是一个指向int类型的常量指针 `int * const cpi`

`const int *pci;`  
1. pci是个变量    `pci`
2. pci是个指针变量  `* pci`  
3. pci是一个指向整数的指针变量 `int * pci`
4. pci使一个指向整数常量的指针变量 `const int *pci`

### 指向常量的指针
指针指向常量，不能通过指针修改它引用的值   

```   
int num=5;
const int limit=500;
int *pi;
const int *pci;

pi=&num;//指向整数
pci=&limit;//指向整数常量，不能通过解引用指针的方式修改limit     
```   
但是可以修改指针，指针不是常量，pci可以指向另一个变量，只是允许的      

### 指向非常量的常量指针  
指针是常量，指向的不是常量   
也就是说指针一旦赋值就不可以修改，但是它所指向的值是可以修改的    

```
int num;
int * const cpi=&num;
```

指针指向的变量不可以改变，但是指针指向变量的值是可以修改的    

### 指向常量的常量指针   
`const int * const cpci =&limit;`   

`* const cpic` 常量指针，`const int `表示指向常量    

不能修改指针，也不能修改指针指向的数据     

### 指向 "指向常量的常量指针" 的指针   

```
const int limit=300;//常量
const int * const cpci=&limit;//指向常量的常量指针
const int * const * pcpci=&cpci;指向 "指向常量的常量指针" 的指针 
```

`const int * const * pcpci;`  
1. pcpci 是一个变量     pcpci
2. pcpci是一个指针变量   *pcpci
3. pcpci是一个指向 `const int * const ` 类型的指针变量    
4. 拆分 `const int * const ` 
5. const 表示常量 ，`* const `表示常量指针， `const int `表示指向常量，最后合并`const int * const`表示指向常量的常量指针  
6. 所以总的就是** 指向 "指向常量的常量指针" 的指针 **  





## 内存分配   
### malloc
使用`malloc`分配内存，返回一个指针，在使用完之后要记得释放，否则修改指针之后，就无法找回这段内存的首地址，也就没办法回收了。  
ps: 昨天做题碰到c++ 11 提供的智能指针 unique_ptr 貌似在修改之后会自动释放掉指针    
`void* malloc(size_t)`   
很简单的一个函数    
`char * p=(char *)malloc(100*sizeof(char))`    
malloc()里面通常用单位数量*类型大小(sizeof(type)),malloc返回的是void* 万能指针，所以需要类型强转来使用    

### calloc  
malloc 在分配内存的时候并不会修改内存区域，所以内存中存在许多垃圾；calloc在分配内存的时候会清空内存   
`void *calloc(size_t numElements,size_t elementSize)`     
第一个参数表示单位个数，第二个表示类型大小    
`char *p=(char*)calloc(100,sizeof(char));`

### realloc  
realloc 用来修改分配内存的大小   
`void *realloc(void *ptr,size_t size);`   

|第一个参数|第二个参数|行为|
|----|----|----|
|空|无|同malloc|
|非空|0|原内存块释放|
|非空|比原内存块小|利用当前的块分配更小的块|
|非空|比原内存块大|要么在当前位置要么在其他位置分配更大的块|     

ps:前几天看glibc堆内存分配，那么最后一个分配更大的内存块时，在分配内存的时候，假如周围有足够的内存，那么就直接扩展内存，使用的是sbrk系统调用，否则就使用mmap系统调用。sbrk是直接扩展，mmap是映射出去   

### alloca函数和变长数组   
alloca在函数的栈帧上分配内存，函数返回之后会自动释放内存  
有的系统底层运行不基于栈，所以alloca很难实现，所以尽量别用    

变长数组就是数组的大小是有变量决定的，但是数组的大小一旦决定，就无法改变，其实变长数组的用法很常见，但是有的教材可能说这种用法是违法的，但其实他是c99引进的，所以教材可能太古老了....
```
int n;
cin>>n;
int temp[n];//temp数组的大小为n，之后不能改变了
```

### free释放内存   
`void free(void *ptr);`   
so easy      
释放完了之后，尽量把指针赋值为NULL，这样就可以避免之后误用 

### 重复释放
重复释放一段内存会报错，所以释放一遍之后尽可能把指针赋值为NULL      

ps：两次释放一段内存会报错，但是glibc中有个double free的漏洞，可以绕过两次释放的检查，可以把一段内存的放到freebin(空闲内存链表)里面两次，所以再次请求内存时，会得到两个指针指向同一个内存空间  

```
//test double free
#include<stdlib.h>
#include<stdio.h>

int main()
{
        char *a =(char *)malloc(10);
        printf("a: %p\n",a);
        char *b=(char *)malloc (10);
        char *c=(char *)malloc(10);
        printf("b: %p\n",b);
        printf("c: %p\n",c);
        free(a);
        free(b);//绕过二次释放的检查
        free(a);

        printf("------------after free----------------------\n");
        char *d=(char *)malloc(10);

        printf("d: %p\n",d);
        char *e=(char*)malloc(10);
        printf("e: %p\n",e);
        char *f=(char*)malloc(10);
        printf("f: %p\n",f);
}
```

```
a: 0x729010
b: 0x729240
c: 0x729260
------------after free----------------------
d: 0x729010
e: 0x729240
f: 0x729010
```
最后d和f的内存地址一样的     


## 指针和函数   
函数指针的具体使用可以看我的[另一篇博客]({{site.url}}/2018/06/15/function_pointer/)        
堆通常用来管理动态内存，堆也通常是全局变量的存储位置；栈帧(stack frame)通常用来存放程序存放函数参数和局部变量       
注意: 不能向函数内传入常量的引用，因为`&`只能取左值的引用，不能取右值       

## 指针和数组  
微机原理中学到过：内存中的地址是线性的，所以说二维数组的实现也是在一维数组的基础上实现的，就是映射   
![](http://or4d8nhvk.bkt.clouddn.com/18-7-26/39800052.jpg)    

### 指针表示法和数组表示法
```
int arr[5];
int *arr2=arr;
```
arr[i]和*(arr2+i)的值是相等的      
但是他们的机器码是不一样的:  
>  arr[i]生成的机器码是从arr位置开始，移动i个位置，取出内容。而*(arr2+i)是从arr开始，在地址上增加i，然后取出地址中的内容     


传递一维数组有两种方式 ：
1. void passarray(int a[]) //数组表示法
2. void passarray(int *a)  //指针表示法
函数的内部可以使用数组或是指针访问数组     

传递多维数组：     
使用指针表示法传递多维数组的时候，一定要传递数组的形态(列数),也就是说除了第一维以外，其他维度的长度都需要指明，因为我们需要多维数组的列数来映射确定的内存，如下图中arr[1] arr[2]...
![](http://or4d8nhvk.bkt.clouddn.com/18-7-26/39800052.jpg)       
1. void passdoublearray(int arr[][5],int rows)
2. void passdoublearray(int (*arr)[5],int rows)


## 字符串数组 
`char temp[10];` temp可以用来存10个字符，不过一般用最后一个字符位存放字符串结尾'\0'  

## 指针与数据结构
链表啊，队列啊，堆栈啊，树啊之类的都可以实现，不提了   


## 安全和注意问题 
`int * a1,a2;`   
上面的生命中a1是一个指针，a2是一个int变量       
### sizeof 指针
sizeof(a1);//结果是4  指针其实就是一个int数字，所以只占四个字节     
### 指针运算   
```
int *a1;
a1++;//指针进行运算的时候会自动根据数据类型的长度和运算操作数修改指针   
```

### 结构体内存填充  

结构体内存的对齐可能使得内存连续，所以结构体的总内存有可能大于每个字段占用内存之和。
内存连续的结构体可以使用**指针运算**有可能访问结构体中的不同字段，但是有风险，还是尽量直接使用字段访问结构体中不同字段数据    
![](http://or4d8nhvk.bkt.clouddn.com/18-7-26/94298278.jpg)   




