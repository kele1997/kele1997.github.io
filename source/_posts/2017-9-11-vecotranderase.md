---
layout: post
author: kele
title: "一元多项式的乘法与加法运算（20 分）"
tags:
  - 总结
  - 学习使我快乐
date: 2017-9-11
---
# 一元多项式的乘法与加法运算（20 分）
```
//初版18分，
／*

测试点	提示	结果	耗时	内存
0	sample换个数字	答案正确	3 ms	244KB
1	同类项合并时有抵消	答案正确	2 ms	240KB
2	系数和指数取上限，结果有零多项式	答案正确	2 ms	244KB
3	输入有零多项式和常数多项式	段错误	2 ms	240KB
*/
#include <iostream>
#include <vector>
#include<algorithm>
using namespace std;

typedef struct {
    int zhishu;
    int xishu;
}danxiang;

bool cmp(danxiang a,danxiang b)
{
    return a.zhishu>b.zhishu;
}

int main()
{
    vector<danxiang> a,b,jiafa,chengfa;
    int n,zhishu,xishu;
    cin>>n;


        while(n--)
        {
            danxiang tmp;
            cin>>tmp.xishu>>tmp.zhishu;
            a.push_back(tmp);
        }
        cin>>n;
        while(n--)
        {
            danxiang tmp2;
            cin>>tmp2.xishu>>tmp2.zhishu;
            b.push_back(tmp2);
        }
        //for(int i=0;i<a.size();i++)
        //    cout<<a[i].zhishu<<a[i].xishu;
        //乘法

        for(int i=0;i<a.size();i++)
        {
            for(int j=0;j<b.size();j++)
            {
                danxiang tmp3;
                tmp3.xishu=a[i].xishu*b[j].xishu;
                tmp3.zhishu=a[i].zhishu+b[j].zhishu;
                chengfa.push_back(tmp3);
            }
        }
        //排序+合并同类项
        sort(chengfa.begin(),chengfa.end(),cmp);
        int tmp;
        if(chengfa.size()>1)
        {
            for(vector<danxiang>::iterator i=chengfa.begin()+1;i!=chengfa.end();i++)
            {
                if(i->zhishu==(i-1)->zhishu)
                {
                    (i-1)->xishu+=i->xishu;
                    chengfa.erase(i);
                }
            }
        }




        int flag=0;
        for(int i=0;i<chengfa.size();i++)
        {
            if(flag&&chengfa[i].xishu!=0) {
                cout << " ";
            }
            if(chengfa[i].xishu!=0) {
                cout << chengfa[i].xishu << " " << chengfa[i].zhishu;
                flag = 1;
            }
        }


        if(flag==0)
            cout<<"0 0";
        cout<<endl;
        //加法
        for(int i=0;i<a.size();i++)
            jiafa.push_back(a[i]);
        for(int i=0;i<b.size();i++)
            jiafa.push_back(b[i]);

        int cnt=0;
        if(jiafa.size()>1)
        {
            for(vector<danxiang>::iterator i=jiafa.begin();i!=jiafa.end()-1;i++)
            {
                for(vector<danxiang>::iterator j=i+1;j!=jiafa.end();j++)
                {
                    if(i->zhishu==j->zhishu)
                    {
                        i->xishu+=j->xishu;
                        cnt++;
                        jiafa.erase(j);
                    }
                }
            }
        }

        sort(jiafa.begin(),jiafa.end(),cmp);


        flag=0;
        for(int i=0;i<jiafa.size();i++)
        {
            if(flag&&jiafa[i].xishu!=0) {
                cout<< " ";
            }
            if(jiafa[i].xishu!=0)
            {
                cout << jiafa[i].xishu << " " << jiafa[i].zhishu;
                flag=1;
            }
        }

        if(flag==0)
            cout<<"0 0";




    return 0;

}
```
终结版
```
#include <iostream>
#include <vector>
#include<algorithm>
using namespace std;

typedef struct {
    int zhishu;
    int xishu;
}danxiang;

bool cmp(danxiang a,danxiang b)
{
    return a.zhishu>b.zhishu;
}

int main()
{
    vector<danxiang> a,b,jiafa,chengfa;
    int n,zhishu,xishu;
    cin>>n;


        while(n--)
        {
            danxiang tmp;
            cin>>tmp.xishu>>tmp.zhishu;
            a.push_back(tmp);
        }
        cin>>n;
        while(n--)
        {
            danxiang tmp2;
            cin>>tmp2.xishu>>tmp2.zhishu;
            b.push_back(tmp2);
        }
        //for(int i=0;i<a.size();i++)
        //    cout<<a[i].zhishu<<a[i].xishu;
        //乘法

        for(int i=0;i<a.size();i++)
        {
            for(int j=0;j<b.size();j++)
            {
                danxiang tmp3;
                tmp3.xishu=a[i].xishu*b[j].xishu;
                tmp3.zhishu=a[i].zhishu+b[j].zhishu;
                chengfa.push_back(tmp3);
            }
        }
        //排序+合并同类项
        sort(chengfa.begin(),chengfa.end(),cmp);
        int tmp;
        if(chengfa.size()>1)
        {
            for(vector<danxiang>::iterator i=chengfa.begin()+1;i!=chengfa.end();i++)
            {
                if(i->zhishu==(i-1)->zhishu)
                {
                    (i-1)->xishu+=i->xishu;
                    i->xishu=0;
                }
            }
        }




        int flag=0;
        for(int i=0;i<chengfa.size();i++)
        {
            if(flag&&chengfa[i].xishu!=0) {
                cout << " ";
            }
            if(chengfa[i].xishu!=0) {
                cout << chengfa[i].xishu << " " << chengfa[i].zhishu;
                flag = 1;
            }
        }


        if(flag==0)
            cout<<"0 0";
        cout<<endl;
        //加法
        for(int i=0;i<a.size();i++)
            jiafa.push_back(a[i]);
        for(int i=0;i<b.size();i++)
            jiafa.push_back(b[i]);

        if(jiafa.size()>1)
        {
            for(vector<danxiang>::iterator i=jiafa.begin();i!=jiafa.end()-1;i++)
            {
                for(vector<danxiang>::iterator j=i+1;j!=jiafa.end();j++)
                {
                    if(i->zhishu==j->zhishu)
                    {
                        i->xishu+=j->xishu;
                        j->xishu=0;
                    }
                }
            }
        }

        sort(jiafa.begin(),jiafa.end(),cmp);


        flag=0;
        for(int i=0;i<jiafa.size();i++)
        {
            if(flag&&jiafa[i].xishu!=0) {
                cout<< " ";
            }
            if(jiafa[i].xishu!=0)
            {
                cout << jiafa[i].xishu << " " << jiafa[i].zhishu;
                flag=1;
            }
        }

        if(flag==0)
            cout<<"0 0";




    return 0;

}
```
## 段错误的原因总结
要理解段错误必须了解vector::erase()函数的使用方法，这个函数删除一个元素，并且反而下一个元素的迭代器。但是注意，他的参数就是一个迭代器，所以他删除元素之后，直接修改了迭代器，造成了vector访问越界 ，产生segmentfault错误。  
可以通过下面的代码理解一下:  
    #include <iostream>
    #include <vector>
    using namespace std;

    int main()
    {
        vector<int> a;
        for(int i=0;i<10;i++)
            a.push_back(i);
        /*
        for(vector<int>::iterator i=a.begin();i!=a.end();i++)
            cout<<"地址: "<<&i<<" "<<"数值: "<<*i<<endl;
            */
        for(vector<int>::iterator i=a.begin();i!=a.end();i++)
        {
            cout<<"地址: "<<&i<<" "<<"数值: "<<*i<<endl;
            a.erase(i);
            cout<<"使用erase之后: "<<&i<<" "<<*i<<endl;
        }
        cout<<"删除之后的"<<endl;
        for(vector<int>::iterator i=a.begin();i!=a.end();i++)
            cout<<*i<<endl;
    }

ps：可以看到迭代器i的地址在一个循环中始终不变，所以说i作为一个迭代器和指针类似，但是并不是指针，可能是他的数据区才用来指向迭代数据的地址
