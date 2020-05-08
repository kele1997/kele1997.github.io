---
layout: post
tilte: "leetcode题解实践"
author: "kele"
date: 2017-8-9
catalog: true
tags:
  - 算法
  - 学习使我快乐
  - 总结


---
# leetcode题解实践笔记
## Pascal's Triangle
帕斯卡三角，当然也叫杨辉三角，某一行某一列的元素等于上一行相邻两个元素的和，所以说找规律就成了,但是重点在与vector的初始化容量，否则就会报错，提示你`reference binding to null pointer of type 'struct value_type'`
```

class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>>res;
        res.resize(numRows);
        int temp;
        for(int i=0;i<numRows;i++)
        {
            for(int j=0;j<i+1;j++)
            {
                if(j==0||j==i)
                {
                    temp=1;
                    res[i].push_back(temp);
                }
                
                else if(i>=1&&j>=1)
                {
                    temp=res[i-1][j]+res[i-1][j-1];
                     res[i].push_back(temp);
                }
                   
            }
        }
        return res;
    }
};

```

运行时间3ms，看了大佬的做法，0ms，我的使用了太多的判断语句，耗时会变得很多..........
```
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>>res;
        if(numRows==0)
            return res;
        
        res.resize(numRows);
        for(int i=0;i<numRows;i++)
            res[i].resize(i+1);
        res[0][0]=1;
        for(int i=0;i<numRows;i++)
        {
            res[i][0]=1;
            res[i][i]=1;
        }
        for(int i=2;i<numRows;i++)
        {
          for(int j=1;j<i;j++)
              res[i][j]=res[i-1][j]+res[i-1][j-1];
        }
        
        return res;
    }
};
```
但是感觉题解上的答案更节省时间，因为只用了一个二次循环

```
class Solution {
public:
vector<vector<int> > generate(int numRows) {
vector<vector<int> > vals;
vals.resize(numRows);
for(int i = 0; i < numRows; i++) {
    vals[i].resize(i + 1);
    vals[i][0] = 1;
    vals[i][vals[i].size() - 1] = 1;
        for(int j = 1; j < vals[i].size() - 1; j++) {
        vals[i][j] = vals[i - 1][j - 1] + vals[i - 1][j];
        }
}
return vals;
}
};
```