---
layout: post
author: "kele"
date: "2018-05-19 20:05:10"
title: "leetcode 9 Palindrome Number"  
katex: true
tags: 
  - leetcode
---
# leetcode 9 Palindrome Number  
## 方法一
判断一个数字是否是回文数字，我先转换成字符串，然后从中心向两头比较，好久没写了，有点蛋疼。。。。。 
```
class Solution {
public:
    bool isPalindrome(int x) {
        string number=to_string(x);
        int length=number.length();
        int center=length/2;
        if(length%2)
        {
            for(int i=1;i<=center;i++)
            {
                if(number[center-i]!=number[center+i])
                    return false;
            }
        }
        else
        {
            for(int i=1;i<=center;i++)
            {
                if(number[center-i]!=number[center+i-1])
                    return false;
            }
        }
        return true;
    }
};
```  


看到另一种做法先判断特殊情况，例如x小于0,那么x一定不是回文数字，假如x不等于0，并且x%10==0,那么x的结尾一定有0，开头不能有0,因此x也不是回文数字。对于一般情况，我们把x的后半部分数字提取出来，判断时候相等就可以了，但有时候数字的长度是奇数，所以我们有时候需要判断一下 

```
class Solution {
public:
     bool isPalindrome(int x) {
        if(x<0||(x!=0&&x%10==0)) 
            return false;
        int sum=0;
        while(x>sum)
        {
            sum+=sum*10+x%10;
            x/=10;
        }
        return (x==sum)||(x==sum/10);
    }
};
```


上面的运行时间在290ms，研究了下提交中运行时间最短的代码
。。。。。。
// 这种全局执行函数，纯粹是为了刷排名
static const auto _______ = []() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
    return nullptr;
}();

发现这个东东，主要是关闭cin的缓冲，解除cin和stdio的绑定，提高运行时间的，其实运行的算法没有啥区别，不做研究了2333


## 方法二:manacher算法
方法二主要是在方法一的基础上进行了相关的改进:    
1. 在每个字符的前面和后面都插入`#`字符，在字符串的开头和结尾分别添加一个`^`和`$`表示开头和结尾  
2. 记录下回文串的长度，感觉像是动态规划的算法    

```
class Solution {
public:
    string preprocess(string a)
    {
        int n=a.length();//string的赋值一定要用双引号，不然出现一些奇怪的问题，稍后总结
        if(n==0) return "^$";
        string ret="^";
        for(int i=0;i<n;i++)
        {
            ret+="#"+a.substr(i,1);//直接取下标不成功，只能使用substr
        }
        ret+="#$";
        return ret;
    }
    
    string longestPalindrome(string s) {
        string T=preprocess(s);
        int n=T.length();
        int *p=new int [n];//用于记录当前点出回文串的长度
        if(n==0) return "";
        int c=0,r=0;//c用来记录当前最长子回文串的长度，r记录当前最长子回文串的右端点
        int i_mirror;
        for(int i=1;i<n-1;i++)
        {
            i_mirror=2*c-i;//找到i关于当前最长子回文串中心c的对称点
            p[i]=r>i?min(p[i_mirror],r-i):0;//如果当前要计算的点在最长子回文串内，那么我们看他的对称点出的子回文串长度，要计算点出的回文串至少是min(p[i_mirro],r-i)，要防止超过最长子回文串的右端点;否则就是在最长子回文串外，需要赋值为0,一位一位的扩展(从中心向两头扩展)
            
            
            while(T[i+1+p[i]]==T[i-1-p[i]])//从中心向两端扩展
                p[i]++;
            
            if(i+p[i]>r)//更新最大子回文串的中心和长度
            {
                c=i;
                r=i+p[i];
            }
        }
        int maxlen=0,center=0;//遍历整个数组，找出最大的子回文串,其实下面的循环可以放到上面里去
        for(int i=1;i<n-1;i++)
        {
            if(p[i]>maxlen)
            {
                center=i;
                maxlen=p[i];
            }
        }
        return s.substr((center-maxlen-1)/2,maxlen);//因为添加了许多'#'，所以位置需要做一些改变
    }
    
    
};
```

参考链接:  https://articles.leetcode.com/longest-palindromic-substring-part-ii/