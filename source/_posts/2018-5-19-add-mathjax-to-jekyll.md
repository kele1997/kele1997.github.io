---
layout: post
author: "kele"
date: "2018-05-19 22:42:37"
title: "给jekyll 添加mathjax"
katex: true
tags:
  - jekyll
---
# 给jekyll 添加 mathjax  
准备刷leetcode，算法题目自然少不了复杂度，$N^{2}$之类数学公式的数字，因此需要添加mathjax来显示数学公式，试了不少的坑，记录一下下:    

首先是一个dollar符不显示公式，这是因为mathjax考虑到 `$` 比较常用，因为外国人的货币都是用 `$` 来表示的嘛，所以默认关闭了使用`$..$`表示公式的方法，而是采用 `$$....$$`,但是可以手动修改:     

```
    <!-- mathjax-->
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({tex2jax: {inlineMath: [ ['$','$'] ],
        displayMath: [ ['$$','$$'] ],
        processEscapes: true,}});
    </script> -->
<script src="http://or4d8nhvk.bkt.clouddn.com/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    
```
手动配置displaymath和inlinemath,然后把processescapes转义打开     

其次是行内公式还是展示公式：    
行内公式: $x=y^{2}$ 
展示公式: 

$$x=y^{2}$$    

其实呢,我使用的jekyll 的markdown引擎是kramdown,kramdown默认是支持mathjax，但是mathjax默认是使用`$$....$$`来同时表示行内公式和展示公式的，[详情参见](https://stackoverflow.com/questions/25146431/how-to-use-mathjax-in-jekyll)    

但是我手动测试发现一个问题,其实全用`$$`有的公式会被解析成为行内公式，有的会被解析为展示公式(kramdown会把长的行内公式转化成展示公式)，所以假如你不想用上面的配置，使用默认配置的话，在你想用展示公式的时候在公式后面加一个回车换行符就可以了，举个栗子:    
```
$$ 
y=x^{2}
$$
```

$$
y=x^{2}
$$


注意: 展示公式的时候要在公式要空出公式的上下两行，否则会成为行内公式；另外假如你想输入\\$的时候，使用反斜杠转义就可以了(两个反斜杠)

mathjax 使用参考:
https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference