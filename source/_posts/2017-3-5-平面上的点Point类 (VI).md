---
layout: post
author: "可乐"
date: 2017-3-5 13:33:00
title: "平面上的点——Point类 (VI)"
tags:
  -  OJ
  -  学习使我快乐
---
	Description
	
	在数学上，平面直角坐标系上的点用X轴和Y轴上的两个坐标值唯一确定。现在我们封装一个“Point类”来实现平面上的点的操作。
	
	根据“append.cc”，完成Point类的构造方法和接口描述中的方法和函数。
	
	接口描述：
	showPoint()函数：按输出格式输出Point对象。
	Point::show()方法：按输出格式输出Point对象。
	Point::showSumOfPoint()方法：按格式输出程序运行至当前存在过的Point对象总数。
	Point::x()方法：取x坐标。
	Point::y()方法：取y坐标。
	Point::x(double)方法：传参数设置x坐标并返回。
	Point::y(double)方法：传参数设置y坐标并返回。
	Point::setPoint(double,double)方法：设置Point对象的x坐标（第一个参数）和y坐标（第二个参数）并返回本对象。
	Point::isEqual()方法：判断传入的参数与对象的坐标是否相同，相同返回true。
	Point::copy()方法：传参数复制给对象。
	Point::inverse()方法，有两个版本：不传参数则将对象自身的x坐标和y坐标互换；若将Point对象做参数传入，则将传入对象的坐标交换复制给对象自身，不修改参数的值。
	
	Input
	
	输入多行，每行为一组坐标“x,y”，表示点的x坐标和y坐标，x和y的值都在double数据范围内。
	
	Output
	
	用ShowPoint()函数来输出（通过参数传入的）Point对象的值或坐标值：X坐标在前，Y坐标在后，Y坐标前面多输出一个空格。每个坐标的输出精度为最长16位。
	对每个Point对象，调用show()方法输出其值，输出格式与ShowPoint()函数略有不同：“Point[i] :”，i表示这是程序运行过程中第i个被创建的Point对象。
	调用showSumOfPoint()输出Point对象的计数统计，输出格式见sample。
	
	C语言的输入输出被禁用。
	
	Sample Input
	
	1,2
	3,3
	2,1
	Sample Output
	
	Point[3] : (1, 2)  
	Point[1] : (2, 1)   
	Point[4] : (3, 3)   
	Point[1] : (3, 3)   
	Point[5] : (1, 2)   
	Point[1] : (1, 2)   
	Point[2] : (0, 0)  
	==========gorgeous separator==========  
	Point[2] : (-7, 5)  
	Point[3] : (1, 2)  
	Point[4] : (3, 3)   
	Point[5] : (1, 2)   
	Point[6] : (-7, 5)   
	==========gorgeous separator==========  
	Point[63] : (3, 3)   
	Point : (3, 3)   
	Point : (3, 3)   
	Point : (3, 3)  
	In total : 64 points.   
	HINT  
	
	给函数正确的返回值。注意常量对象调用的函数。

   
## 代码如下：
![](./img/25.jpg)

	
	#include <iostream>
	#include <iomanip>
	using namespace std;
	
	using namespace std;
	class Point{
	private:
	    double m,n;
	    int id;
	    static int sum;
	public:
	    double x()const{return m;}
	    double y()const{return n;}
	    double x(double a){m=a;return m;}
	    double y(double b){n=b;return n;}
	    double getX(){return m;}
	    double getY(){return n;}
	    double setX(double a){return m=a;}
	    double setY(double b){return n=b;}
	    Point& setPoint(double a,double b){m=a;n=b;return *this;}
	    Point(double a=0):m(a),n(a){sum++;id=sum;}
	    Point(double a,double b):m(a),n(b){sum++;id=sum;}
	    bool isEqual(Point &p)const{if(p.m==m&&p.n==n)return true;else return false;}
	    Point& copy(Point &p){m=p.m;n=p.n;return *this;}
	    Point& inverse(){double tmp;tmp=m;m=n;n=tmp;return *this;}
	    Point& inverse(Point &p){m=p.n;n=p.m;return *this;}
	    void show()const{cout<<setprecision(16)<<"Point["<<id<<"] : ("<<m<<", "<<n<<")"<<endl;}
	    static void showSumOfPoint(){cout<<setprecision(16)<<"In total : "<<sum<<" points."<<endl;}
	};
	int Point::sum=0;
	
	
	void ShowPoint(const Point &p)
	{
	    cout<<std::setprecision(16)<<"Point : ("<<p.x()<<", "<<p.y()<<")"<<endl;
	}
	
	void ShowPoint(double x, double y)
	{
	    ///Point p(x, y);
	    cout<<std::setprecision(16)<<"Point : ("<<x<<", "<<y<<")"<<endl;
	}
	
	void ShowPoint(Point &p, double x, double y)
	{
	    cout<<std::setprecision(16)<<"Point : ("<<p.x(x)<<", "<<p.x(y)<<")"<<endl;
	}
	int main()
	{
	    int l(0);
	    char c;
	    double a, b;
	    Point p, q, pt[60];
	    while(std::cin>>a>>c>>b)
	    {
	        if(a == b)
	            p.copy(pt[l].setPoint(a, b));
	        if(a > b)
	            p.copy(pt[l].setPoint(a, b).inverse());
	        if(a < b)
	            p.inverse(pt[l].setPoint(a, b));
	        if(a < 0)
	            q.copy(p).inverse();
	        if(b < 0)
	            q.inverse(p).copy(pt[l]);
	        pt[l++].show();
	        p.show();
	    }
	    q.show();
	    cout<<"==========gorgeous separator=========="<<endl;
	    double x(0), y(0);
	    for(int i = 0; i < l; i++)
	        x += pt[i].x(), y -= pt[i].y();
	    pt[l].x(y), pt[l].y(x);
	    q.copy(pt[l]).show();
	    for(int i = 0; i <= l; i++)
	        pt[i].show();
	    cout<<"==========gorgeous separator=========="<<endl;
	    const Point const_point(3, 3);
	    const_point.show();
	    for(int i = 0; i <= l; i++)
	    {
	        if(const_point.isEqual(pt[i]))
	        {
	            ShowPoint(const_point);
	            ShowPoint(const_point.x(), const_point.y());
	            ShowPoint(Point(const_point.x(), const_point.y()));
	        }
	    }
	    const_point.showSumOfPoint();
	}
