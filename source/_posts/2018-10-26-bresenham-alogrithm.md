---
layout: post
author: "kele"
date: "2018-10-26 21:01:56"
title: "Bresenham 算法（计算机图形学基础实验）"
mathjax: true
music: "1318288369"
tags: 
  - 计算机图形学
---
# Bresenham 算法
因为使用c/c++的话，图形库要么太大材小用，要么就是太古老，因此决定使用python 的pillow库来实现文章中的算法，但是后来发现pyplot库比较形象


## 概述
我们都知道显示器显示的画面都是由像素组成的，像素都是一个一个小的方块组成的，而我们之所以感觉不到方块的存在的原因是像素方块足够小，同样大小的屏幕，像素点越小，也就是分辨率越高，画面也更好。那么计算机是怎样画出一条直线的呢？ 
![](https://upload.wikimedia.org/wikipedia/commons/d/d4/Bresenham.png)
上面图就是我们平时看到的直线的像素组成，一条直线其实是由许多像素方块组成的，那么怎么计算涂那些像素方块，才可以让最后的“直线”更像直线呢？   

按照正常思路，一条直线的方程是点斜式方程，\(y=kx+b\)    

1. $\(x_{i+1}=x_i+xStep\)$
2. $\(y_{i+1}=y_i+yStep\)$ 或者 $\(y_{i+1}=y_i+k*xStep\)$

>如果 Δx > Δy ，说明x轴的最大差值大于y轴的最大差值，x轴方向为步进的主方向，xStep = 1，yStep = k；
>如果 Δy> Δx，说明y轴的最大差值大于x轴的最大差值，y轴方向为步进的主方向，yStep = 1，xStep = 1 / k。

这就是计算机绘制直线最简单的算法 DDA 算法  
因此xStep或者yStep有可能产生浮点数，别小看这个浮点数，当我们屏幕分辨率高的时候，浮点数的计算量会变成显示像素的瓶颈，所以Bresenham算法应运而生，Bresenham主要是没有产生浮点数运算，全部都是整数加法运算，也是步进的思想



## Bresenham 绘制直线算法
>中点Bresenham绘制二维直线段的基本原理是每次在最大的位移方向上走一步，而另一个方向是走步还是不走步要取决于误差项的判别    

首先设直线方程为：
$$F(x,y)=y-kx-b=0 ,其中 k=\frac{\Delta{x}}{\Delta{y}}$$     

这条直线将平面分为三个部分：  
1. 对于在直线上的点： $F(x,y)=y-kx-b=0$
2. 对于直线下方的点： $F(x,y)=y-kx-b<0$
3. 对于直线上方的点： $F(x,y)=y-kx-b>0$

那么我们用步进法来推到整条直线，将 x 坐标加1,将 y 坐标加 0.5,判断这个点与直线的位置，如果在直线上，直接绘制；如果在直线下方，将y+1;如果在直线上方，那么y不变

$d=F(x_i+1,y_i+0.5)=y_i+0.5-k(x_i+1)-b$

$$y=\begin{cases}
y+1,d<0\\[2ex]
y,d>=0
\end{cases}$$

利用代数推导： 
$d_i=F(x_i+1,y_i+0.5)$ 
1. $k=\frac{\Delta{y}}{\Delta{x}}$
2. 当 d<0时，x=x+1,y=y+1 所以(x+1,y+0.5) ==> (x+2,y+1.5),求得 $$\begin{equation}\begin{split}d_{i+1}=&F(x_i+2,y_i+1.5)\\&=y_i+1.5-k(x_i+2)-b\\&=d_i+1-k\end{split}\end{equation}$$
3. 当 d>0时，x=x+1,y=y 所以 (x+1,y+0.5)==>(x+2,y+0.5) 求得 $d_{i+1}=F(x_i+2,y_i+0.5)=d_i-k$
4. d的初始值 $d_0=F(x_i+1,y_i+0.5)=y_0-kx_0-b-k+0.5=0.5-k$ 因为初始位置在直线上，所以 $y_0-kx_0-b=0$

因为上面的代数中存在一个0.5，有浮点数运算，耗费资源，所以我们统一将d乘以$2*\Delta{x}$

算法流程: 
0≤k≤1时Bresenham算法的算法步骤为
1. 输入直线的两端点P0(x0,y0)和P1(x1,y1)。
2. 计算初始值△x、△y、d=△x-2△y、x=x0、y=y0。
3. 绘制点(x,y)。判断d的符号。
若d<0，则(x,y)更新为(x+1,y+1)，d更新为△xd+2△x-2△y；否则(x,y)更新为(x+1,y), d更新为△xd-△y。
4. 当直线没有画完时，重复步骤3。否则结束。


实验的时候d*2$\Deltax$之后，误差会变大，因此取消
```python
def draw_a_line_bresenham(x0,y0,x1,y1,ax,k):
    x=x0
    y=y0
    dx=x1-x0
    dy=y1-y0
    #d=dx-2*dy
    d=0.5-k
    while x<x1:
        add_pixel(x,y,ax,1)
        if d<=0:
            d+=1-k
            #d=d*dx+dx-dy
            x,y=x+1,y+1
        else:
            d-=k
            #d=dx*d-dy
            x,y=x+1,y   
```





## bresenham 算法绘制圆形
首先了解一下八分法，圆有无数条对称轴，比较好绘制的有四条（上-下，左-右，左上-右下，右上-左下），分为八份的圆中所有点都是关于直线对称的  

首先设圆形方程： 
$$F(x_i,y_i)=x_i^2+y_i^2-R^2=0$$

那么这个圆把平面分为三个部分
1. 点在圆上 F=0
2. 点在圆内 F<0
3. 点在圆外 F>0

我们从（0，R) 开始绘制这个圆，尝试每一步移动一格x，移动半格y (x,y)==>(x+1,y-0.5)     
$$d=F(x_i+1,y_i+0.5)=(x_i+1)^2+(y_i+0.5)^2-R^2$$ 

* d=0 时，画哪个点是人为定义的

$$y=\begin{cases}
y-1,d>=0\\[2ex]
y,d<0
\end{cases}$$

推导公式:
$d_i=F(x_i+1,y_i-0.5)$
1. d<0时，(x+1,y-0.5)==>(x+2,y-0.5)  $$\begin{equation}\begin{split}d_{i+1}&=F(x_i+2,y_i-0.5)\\&=(x_i+2)^2+(y_i-0.5)^2-R^2\\&=d_i+2*x_i+3\end{split}\end{equation}$$
2. d>=0时，(x+1,y-0.5)==>(x+2,y-1.5)   $$\begin{equation}\begin{split}d_{i+1}&=F(x_i+2,y_i-1.5)\\&=(x_i+2)^2+(y_i-1.5)^2-R^2\\&=d_i+2(x_i,y_i)+5\end{split}\end{equation}$$
3. $d_0$的求解: $$\begin{equation}\begin{split}d_0&=F(1,R-0.5)\\&=1+(R-0.5)^2-R^2\\&=1.25-R\end{split}\end{equation}$$

算法流程:  
1. 输入圆的半径R。
2. 计算初始值d=1-R、x=0、y=R。
3. 绘制点(x,y)及其在八分圆中的另外七个对称点。
4. 判断d的符号。若d<0，则先将d更新为d+2x+3，再将(x,y)更新为(x+1,y)；否则先将d更新为d+2(x-y)+5，再将(x,y)更新为(x+1,y-1)。
5. 当x<y时，重复步骤3和4。否则结束。

```python
#八分圆中的八个点
def draw_circle_eigth(x,y,cx):
    add_pixel(x,y,cx,1)
    add_pixel(x,-y,cx,1)
    add_pixel(-x,y,cx,1)
    add_pixel(-x,-y,cx,1)
    add_pixel(y,x,cx,1)
    add_pixel(y,-x,cx,1)
    add_pixel(-y,x,cx,1)
    add_pixel(-y,-x,cx,1)

# 绘制圆
def draw_a_circle(r,cx):
    d=1-r
    x=0
    y=r

    draw_circle_eigth(x,y,cx)
    while x<y:
        if d<0:
            d=d+2*x+3
            x,y=x+1,y
            draw_circle_eigth(x,y,cx)
        else:
            d=d+2*(x-y)+5
            x,y=x+1,y-1
            draw_circle_eigth(x,y,cx)
```




## Bresenham 算法绘制椭圆
设椭圆方程为 $F(x_i,y_i)=b^2x_i^2+a^2y_i^2-a^2b^2$  

$$d=F(x_i+1,y_i-0.5)=b^2(x_i+1)^2+a^2(y_i-0.5)^2-a^2b^2$$

d<=0 $d_{i+1}=d_i+b^2(2*x_i+3)$
d>0  $d_{i+1}=d_i+b^2(2x_i+3)+a^2(-2y_i+2)$
$d_0=b^2+a^2(-b+0.25)

道理差不多，不过多赘述  

算法流程:
1. 输入椭圆的长半轴a和短半轴b。
2. 计算初始值d=b2+a2(-b+0.25)、x=0、y=b。
3. 绘制点(x,y)及其在四分象限上的另外三个对称点。
4. 判断d的符号。若d≤0，则先将d更新为d+b2(2x+3)，再将(x,y)更新为(x+1,y)；否则先将d更新为d+b2(2x+3)+a2(-2y+2)，再将(x,y)更新为(x+1,y-1)。
5. 当b2(x+1)<a2(y-0.5)时，重复步骤3和4。否则转到步骤6。
6. 用上半部分计算的最后点(x,y)来计算下半部分中d的初值：

7. 绘制点(x,y)及其在四分象限上的另外三个对称点。
8. 判断d的符号。若d≤0，则先将d更新为b2(2xi+2)+a2(-2yi+3)，再将(x,y)更新为(x+1,y-1)；否则先将d更新为d+a2(-2yi+3)，再将(x,y)更新为(x,y-1)。
9. 当y>0时，重复步骤7和8。否则结束。

```python
def draw_a_Ellipse(a,b,dx):
    
    d=b*b+a*a*(0.25-b)

    #draw four point on the x,y axis
    add_pixel(a,0,dx,1)
    add_pixel(-a,0,dx,1)
    add_pixel(0,b,dx,1)
    add_pixel(0,-b,dx,1)

    x=0
    y=b
    while b**2*(x*1)<a**2*(y-0.5):
        if d<=0:
            d=d+b**2*(2*x+3)
            add_pixel(x+1,y,dx,1)
            add_pixel(-1-x,y,dx,1)
            add_pixel(-1-x,-y,dx,1)
            add_pixel(x+1,-y,dx,1)

            x+=1
        else:
            d=d+b**2*(2*x+3)+a**2*(2-2*y)
            add_pixel(x+1,y-1,dx,1)
            add_pixel(-x-1,y-1,dx,1)
            add_pixel(x+1,-y+1,dx,1)
            add_pixel(-x-1,-y+1,dx,1)
            y-=1
            x+=1
    
    d=b**2*(x+0.5)**2+a**2*(y-1)**2-a**2*b**2
    x_list=[x,-x]
    y_list=[y,-y]
    
    for i in x_list:
        for j in y_list:
            add_pixel(i,j,dx,1)
    
    while y>0:
        if d<=0:
            d=b**2*(2*x+2)+a**2*(3-2*y)
            add_pixel(x+1,y-1,dx,1)
            add_pixel(-x-1,y-1,dx,1)
            add_pixel(x+1,-y+1,dx,1)
            add_pixel(-x-1,-y+1,dx,1)
            x+=1
            y-=1
        else:
            b=d+a**2*(3-2*y)
            add_pixel(x,y-1,dx,1)
            add_pixel(x,-y+1,dx,1)
            add_pixel(-x,y-1,dx,1)
            add_pixel(-x,-y+1,dx,1)
            y-=1
```

![](http://or4d8nhvk.bkt.clouddn.com/18-10-27/10025241.jpg)
![](http://or4d8nhvk.bkt.clouddn.com/18-10-27/44636706.jpg)



参考链接:  
1. [wikipedia](https://zh.wikipedia.org/wiki/%E5%B8%83%E9%9B%B7%E6%A3%AE%E6%BC%A2%E5%A7%86%E7%9B%B4%E7%B7%9A%E6%BC%94%E7%AE%97%E6%B3%95)
2. [DDA and Bresenham](https://blog.csdn.net/u010429424/article/details/77834046)
3. [python pillow](https://pillow.readthedocs.io/en/latest/reference/Image.html#PIL.Image.new)
4. [DDA 百度百科](https://baike.baidu.com/item/DDA%E7%AE%97%E6%B3%95)
5. [DDA算法 csdn](https://blog.csdn.net/zhaotianyu950323/article/details/79528172)
6. [dda 和 bresenham pylab matplotlib](http://www.cnblogs.com/lepeCoder/p/dda_bresenham.html)
7. [dda 和 bresenham 对比](https://freefeast.info/difference-between/difference-between-dda-line-drawing-algorithm-and-bresenhams-line-drawing-algorithm-dda-algorithm-vs-bresanhams-algorithm/)