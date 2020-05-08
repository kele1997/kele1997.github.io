---
layout: post
title: "shadowsocks服务器的搭建"
date: 2017-7-31
author: kele
catalog: true
tags:
  - 总结
---
# shadowsocks服务器的搭建
最近想要做一些关于netcat命令的题目，但是苦于学校的网络是内网，所以我的netcat命令无法绑定ip,所以只能使用vps,所以我决定使用vps来做。   
首先要有python环境，其次要有pip    
`sudo apt-get install python-gevent python-pip`    
`sudo pip install shadowsocks`    
为了支持某些密码的加密方式可能需要安装`apt-get install python-m2crypto`    
下面是shadowsocks的使用方法：`ssserver --help`     
		usage: ssserver [OPTION]...
		A fast tunnel proxy that helps you bypass firewalls.
		
		You can supply configurations via either config file or command line arguments.
		
		Proxy options:
		  -c CONFIG              path to config file
		  -s SERVER_ADDR         server address, default: 0.0.0.0
		  -p SERVER_PORT         server port, default: 8388
		  -k PASSWORD            password
		  -m METHOD              encryption method, default: aes-256-cfb
		  -t TIMEOUT             timeout in seconds, default: 300
		  --fast-open            use TCP_FASTOPEN, requires Linux 3.7+
		  --workers WORKERS      number of workers, available on Unix/Linux
		  --forbidden-ip IPLIST  comma seperated IP list forbidden to connect
		  --manager-address ADDR optional server manager UDP address, see wiki
		
		General options:
		  -h, --help             show this help message and exit
		  -d start/stop/restart  daemon mode
		  --pid-file PID_FILE    pid file for daemon mode
		  --log-file LOG_FILE    log file for daemon mode
		  --user USER            username to run as
		  -v, -vv                verbose mode
		  -q, -qq                quiet mode, only show warnings/errors
		  --version              show version information
		
		Online help: <https://github.com/shadowsocks/shadowsocks>
然后就可以运行了，但是为了方便我们可以把配置保存在一个json配置文件里面,或者直接像下面一样运行    
`sudo ssserver -s ip地址 -p 服务器端口 -k 密码  -m 加密方式  -t 超时时间   -d start 然后就可以后台运行了,必须要用root权限，否则无法后台运行     
创建一个json配置文件:   

		{
			"server":“*.*.*.*",
			"server_port":1-65535,    #1-1000是系统常用端口，所以你懂得
			"password":"password",
			"method":"aes-256-cfb",  #加密方法，可选择"bf-cfb","aes-256-cfb","des-cfb","rc4",等等。默认是一种不安全的加密，推荐用"aes-256-cfb"
			"timeout":600
			"local_port":1080    #通常shadowsocks客户端的默认端口是1080
		}    

`sudo sssever -c /path/config.json -d start`

成功后台运行
# shadowsocks客户端
我的系统是win10 x64，使用shadowsocksR，配置如下    
![](http://or4d8nhvk.bkt.clouddn.com/17-7-31/38284420.jpg)

## 成功
![](http://or4d8nhvk.bkt.clouddn.com/17-7-31/4696941.jpg)

## 后记
linux上无法连接，把ip地址改成0.0.0.0，成功了。使用公网ip报错,不知道什么原因，官方说公网Ip没有绑定在网卡上面，要绑定0.0.0.0

----

