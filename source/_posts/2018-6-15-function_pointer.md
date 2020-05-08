---
layout: post
author: "kele"
date: "2018-06-15 19:20:33"
title: "函数指针的使用"
tags:
  - c语言
---
# 函数指针的使用
昨天在看go语言中参数声明的方式的博客发现了函数指针。c语言中指针的使用尤为复杂，其中函数指针的用法最实用也最复杂，总结一下函数指针的使用   


## 函数类型和函数指针类型
函数类型就是一个函数，函数指针就是一个指向函数的指针，需要注意的是在我们把一个函数赋值给一个函数指针的时候，会自动把函数类型转型为函数指针   
`int (*p)(int a,int b)=test` //test是一个函数，自动转型   
`int (*p)(int a,int b)=&test` //这种是正常函数指针赋值   

## 函数指针的创建  
`int (*p)(int a,int b);`  
<b>注意 </b> `int *p(int a,int b)`形式是错的，这个形式是声明一个函数，函数名是p，返回值类型是 `int*`,两个参数是都是int类型   
这样就创建了一个函数指针，这个指针的变量名是p，参数是两个int类型的整数，返回值是int类型

```
//a simple demo
#include <stdio.h>
#include <stdlib.h>

int test(int a,int b)
{
    printf("%d\n",a+b);
    return a+b;
}

int main()
{
    int (*p)(int a,int b)=test;
    p(1,2);
    return 0;
}
```

## 参数是函数指针的函数

`void test2(int (*p)(int a,int b),int a,int b)`
这个形式创建了一个函数，函数名是test2,第一个参数是一个函数指针`int (*p)(int a,int b)`,第二个和第三个参数是int类型  

**函数声明也可以丢掉变量名** `void test2(int (*)(int ,int ),int ,int );`  

```
//demo 2
#include <stdio.h>
#include <stdlib.h>

int test(int a,int b)
{
    printf("%d\n",a+b);
    return a+b;
}

void test2(int (*p)(int a,int b),int a,int b)
{
    p(a,b);
}

int main()
{
    //int (*p)(int a,int b)=test;
    //p(1,2);
    test2(test,2,3);
    return 0;
}
```

## 返回值是函数指针类型
`int (*test2())(int a,int b)`   
这样就声明了一个函数，函数名是test2，返回值是一个函数指针，这个函数指针返回值是int，参数是两个int。**注意** 最后面的两个int是函数指针的参数，需要主要的是假如函数的参数为空，最好在函数的参数列表中加一个void,增加可读性   
`int (*test2(void))(int a,int b)`   

```
#include <stdio.h>
#include <stdlib.h>

int test(int a,int b)
{
    printf("%d\n",a+b);
    return a+b;
}


int (*test2())(int a,int b)
{
    int (*p)(int a,int b)=test;
    return p;
}

int main()
{
    //int (*p)(int a,int b)=test;
    //p(1,2);
    //test2(test,2,3);
    test2()(1,2);
    return 0;
}
```

## typedef 函数指针  
```
typedef int (*p1)(int x);//p1是函数指针类型 
typedef int * p2(int x);//a是函数类型(或者说是指向一个函数指针的基类型)
typedef int p3(int x);//b也是函数类型(或者说是指向一个函数指针的基类型)
```

```
#include <stdio.h>
#include <stdlib.h>
typedef int (*p1)(int a,int b);
typedef int *   p2(int a,int b);
typedef int p3(int a,int b);


int test(int a,int b)
{
    printf("%d\n",a+b);
    return a+b;
}

int test1(int a)
{
    printf("%d",a);
    return 1;
}

int main()
{
    p1 tmp1=test;
    tmp1(1,2);
    p2 *tmp2=test;//必须定义函数指针
    tmp2(3,4);
    p3 *tmp3=test;//必须定义函数指针
    tmp3(5,6);
}
```
p2和p3是函数类型，所以在定义的时候一定要声明函数指针，否则会报错`function 'tmp2' is initialized like a variable|`

## 函数指针的数组
`void (*functionarray[10])(void);`    
声明一个函数数组，数组名字叫做functionarray,容量为10,参数是void,返回值为void    

```
//a simple demo for function point array
#include <stdio.h>
#include <stdlib.h>


void test1()
{
    printf("function 1\n");
}

void test2()
{
    printf("function 2\n");
}


int main()
{
    void (*functionarray[2])(void);
    functionarray[0]=test1;
    functionarray[1]=test2;
    for(int i=0;i<2;i++)
    {
        functionarray[i]();
    }
}
```

## 参考链接
[https://blog.csdn.net/nohackcc/article/details/16961691](https://blog.csdn.net/nohackcc/article/details/16961691)    
[http://www-ee.eng.hawaii.edu/~tep/EE160/Book/chap14/subsection2.1.3.1.html](
http://www-ee.eng.hawaii.edu/~tep/EE160/Book/chap14/subsection2.1.3.1.html)  
[https://www.cprogramming.com/tutorial/function-pointers.html](https://www.cprogramming.com/tutorial/function-pointers.html)  


[https://stackoverflow.com/questions/20617067/returning-function-pointer-type](https://stackoverflow.com/questions/20617067/returning-function-pointer-type)   

[https://overiq.com/c-programming/101/typedef-statement-in-c/ ](https://overiq.com/c-programming/101/typedef-statement-in-c/ )


[https://stackoverflow.com/questions/11038430/how-to-create-a-typedef-for-function-pointers](https://stackoverflow.com/questions/11038430/how-to-create-a-typedef-for-function-pointers)  


[https://stackoverflow.com/questions/1591361/understanding-typedefs-for-function-pointers-in-c](https://stackoverflow.com/questions/1591361/understanding-typedefs-for-function-pointers-in-c )


[https://stackoverflow.com/questions/4295432/typedef-function-pointer  ](https://stackoverflow.com/questions/4295432/typedef-function-pointer )


[http://www.learncpp.com/cpp-tutorial/78-function-pointers/  ](http://www.learncpp.com/cpp-tutorial/78-function-pointers/  )

[https://stackoverflow.com/questions/7321993/reference-to-function-syntax-with-and-without  ](https://stackoverflow.com/questions/7321993/reference-to-function-syntax-with-and-without)  