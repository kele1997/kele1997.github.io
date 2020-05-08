---
layout: post
author: "可乐"
title: "RFC 文档时间格式的阅读方法"  
date: "2018-09-24 20:57:35"
tags: 
  - rfc
---
# RFC 文档时间格式的阅读方法
群里突然有小伙伴问起 rfc 822/2822 时间格式，瞬间百度了一下，然后就决定了google一下，搜索了官方的RFC文档，耐心地思考了一下RFC时间格式的阅读方法,做个笔记吧！  

## RFC 2822 date format 
以 RFC 2822 时间格式做个例子:
[文档位置](https://tools.ietf.org/html/rfc2822)   
[php中的时间格式（参考对照）](http://php.net/manual/zh/class.datetime.php)  

```
3.3. Date and Time Specification

   Date and time occur in several header fields.  This section specifies
   the syntax for a full date and time specification.  Though folding
   white space is permitted throughout the date-time specification, it
   is RECOMMENDED that a single space be used in each place that FWS
   appears (whether it is required or optional); some older
   implementations may not interpret other occurrences of folding white
   space correctly.

date-time       =       [ day-of-week "," ] date FWS time [CFWS]  #日期-时间的格式是：  [周几，（可选）] 日期 空白符 时间 空白符

day-of-week     =       ([FWS] day-name) / obs-day-of-week  #以前叫做 day-of-week，现在废弃，改为下面的 day-name 

day-name        =       "Mon" / "Tue" / "Wed" / "Thu" / #周几
                        "Fri" / "Sat" / "Sun"

date            =       day month year  #日期 天 月 年，用空格隔开

year            =       4*DIGIT / obs-year  #日期种的年是一个4位数，这个已经废弃，因为包含在上面了

month           =       (FWS month-name FWS) / obs-month  #月份数，前后都有空白字符，已经废弃，改为下面的month-name了

month-name      =       "Jan" / "Feb" / "Mar" / "Apr" / #月份
                        "May" / "Jun" / "Jul" / "Aug" /
                        "Sep" / "Oct" / "Nov" / "Dec"

day             =       ([FWS] 1*2DIGIT) / obs-day  #天，一个2位整数，已经废弃，包含在date中

time            =       time-of-day FWS zone #时间的格式是： 当日时间 空格 时区

time-of-day     =       hour ":" minute [ ":" second ]  # 当日时间 格式是: 小时:分钟:[秒(可选)]

hour            =       2DIGIT / obs-hour #小时 2位整数，已经废弃，被包含在上面

minute          =       2DIGIT / obs-minute # 分钟，2位整数，已经废弃，被包含在上面了

second          =       2DIGIT / obs-second #秒，2位整数，已经废弃，被包含在上面了

zone            =       (( "+" / "-" ) 4DIGIT) / obs-zone #时区，带加减符号的4位整数，已经废弃，包含在上面了

```

随手看一手php文档中的时间格式:  
`const string RFC2822 = "D, d M Y H:i:s O" ;`
D 代表的是Day of week,d表示 day,M表示Month,Y表示Year,H表示Hour,i表示minute,s表示second,O表示Zone   
>Mon, 15 Aug 2005 15:52:01 +0000
上面就是一个 RFC 2822时间的样例