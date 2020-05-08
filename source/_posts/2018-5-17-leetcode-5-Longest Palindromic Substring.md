---
layout: post
author: "kele"
date: "2018-5-17"
title: "leetcode 5 Longest Palindromic Substring" 
katex: true
tags: 
  - leetcode
---
# leetcode 5 Longest Palindromic Substring  
求最大的子回文字符串   
参考资料:     
https://articles.leetcode.com/longest-palindromic-substring-part-i/    
http://www.leetcode.com/2011/11/longest-palindromic-substring-part-ii.html   

一种 $N^{2}$ 复杂度的算法是，枚举每个字符，然后从这个字符向两边扩展，直到两边不相等为止，这样就找到了一个子回文串了，因此寻找子回文串的最坏情况是 $
\mathcal O(n)$,我们的外循环中需要遍历一遍中心，复杂度又是$\mathcal O(n)$,所以总的复杂度是$\mathcal O(n^{2})$