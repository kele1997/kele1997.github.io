---
layout: post
author: "kele"
date: "2018-07-02 23:27:58"
title: "使用jsp jdbc 连接 mysql数据库"
tags: 
  - java
---
# 使用java jdbc驱动连接数据库
今天做一个jsp注册的小demo，突然就想连接数据库，做一个真正的demo，所以就有了诸多的事情    
jsp连接mysql数据库需要使用jdbc驱动，jdbc驱动的下载地址是[https://dev.mysql.com/downloads/connector/j/](https://dev.mysql.com/downloads/connector/j/),在下面选择platform independent,然后下载第一个或者第二个都是可以的，之后解压压缩包，得到里面的一个** mysql-connector-java-8.0.11.jar ** 文件，这个文件就是我们主要的驱动文件。       


## 准备工作
emmm 假如你出现找不到package的错误，你可以采取下面几个措施(可能有效，滑稽)：   
1. 把mysql-connector-java-8.0.11.jar放到tomcat的lib目录下面
2. 右键工程名，找到properties,找到Java Build Path,在右侧 add External JARS,添加mysql-connector-java-8.0.11.jar  
3. 添加mysql-connector-java-8.0.11.jar到系统环境变量CLASS_PATH中去


## 一个简单的demo
** 注意 ** 国内的有的博客推荐使用这样的demo来测试驱动 
`<%@page import="com.mysql.cj.jdbc.Driver">`    
我在官方的文档demo中发现这种方式不仅不推荐，而且明确标出可能出现问题，而且事实上也确实出现了很大的问题    

首先导入要使用的包: java.sql.DriverManager,java.sql.Connection,java.sql.SQLException,java.sql.Statement,java.sql.ResultSet   

然后使用Class.forName("com.mysql.cj.jdbc.Driver").newInstance();注册驱动，然后创建一个驱动实例    
  





```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>a simple demo for using jdbc</title>
</head>
<body>
<%@page import="java.sql.DriverManager,java.sql.Connection,java.sql.SQLException,java.sql.Statement" %>
<%@page import="java.sql.ResultSet" %>
<%
//1. 注册驱动，创建驱动实例
Class.forName("com.mysql.cj.jdbc.Driver").newInstance();
Connection conn=null;
try {
  //2. 使用驱动连接mysql 服务器
    conn =
       DriverManager.getConnection("jdbc:mysql://localhost/pentest?" +
                                   "user=root&password=root&serverTimezone=GMT%2B8");//必须加上serverTimezone字段，否则失败，貌似是jdbc的一个bug
	Statement stml=null;
	ResultSet rs=null;
  // 3. 创建语句
	stml=conn.createStatement();
  // 4. 执行增删改的语句
  stml.executeUpdate("insert into news values (33,\"test\",\"just a little test\")");//增删改使用executeUpdate
  // 5. 执行查询语句
	rs=stml.executeQuery("select * from news");//查使用executeQuery
    // Do something with the Connection
	//System.out.println(rs.toString());
    
    // 6. 处理查询结果
    while(rs.next())//rs结果集中还有下一行
    {
    	out.print(rs.getString(1)+"    |    ");//输出rs结果的第一列
    	out.print(rs.getString(2)+"    |    ");//输出rs结果的第二列
    	out.println(rs.getString(3)+"<br>");//输出rs结果的第三列
    }
   //...
} catch (SQLException ex) {//错误处理
    // handle any errors
    System.out.println("SQLException: " + ex.getMessage());
    System.out.println("SQLState: " + ex.getSQLState());
    System.out.println("VendorError: " + ex.getErrorCode());
}
%>
</body>
</html>
```


参考链接:
[https://www.cnblogs.com/justlove/p/6946032.html](https://www.cnblogs.com/justlove/p/6946032.html)
