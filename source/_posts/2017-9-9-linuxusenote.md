---
layout: post
author: kele
date: 2017-9-8
catalog: true
title: "linux使用总结"
tags:
  - 总结
  - linux
---
# linux使用总结
![](http://or4d8nhvk.bkt.clouddn.com/17-9-9/83525564.jpg)   
使用了最流行的linux　Mint系统18.2　　
## 使用问题：
* 显卡兼容问题，神船的显卡跑windowsP事没有，但是使用linux，蛋疼地要死要活的，要不就是卡屏，要不就是死机，还好找到了linux　Mint系统，成功上了linux的大船。
* 社交软件。qq上经常哟嗯的qq,微信在linux上都没有，因为腾讯在12年就停止了对linux版本qq的维护，并且禁止低版本qq登陆，但是仍然有解决办法，WineQQ,或者是github学照Mojo:qq(终端qq,高端大气上档次)
* 许多问题不熟悉，无法修改。解决方法就是使用google搜索引擎，不要使用百度就可以解决了

## chrominum无法正常现实embed标签
flash没安装??但是部分网页的flash是可以正常显示的，chrome://flash，发现flash没有安装,但是安装了几次。。。。完全gg，最后找到了解决方法   
`sudo apt-get install pepperflashplugin-nonfree`    

## chrominum指定协议打开应用程序
一开始打开无线网登陆页面的时候，chrome弹出了是否使用x-open,手贱点了确定，再之后，每次登陆的时候都会弹错。。。。     
最终的解决方法是修改配置文件，关闭指定协议打开应用程序   
chrominum的配置在　`用户目录／.config/chromium/Default/Preferences`　我要修改的gwife的协议，于是搜索gbcome，找到之后,把false改为true，false代表启动应用程序，true代表不启动应用程序   

## 更换内核版本
linux最强大的就在与可以一边运行系统，一边访问系统文件，而且基于ubuntu的apt包管理也是超级强大，所以更换内核也可以使用apt 命令更换   
`sudo apt-get install linux-image-??`   　　
安装完新的内核之后一定要记得卸载掉原来的内核，否则就会有两个核，卸载完成之后，记得更新grub   
`sudo update-grub`     
如果更新的grub不正确，或者还原到旧版本的时候出错，看看grub是根据那些文件更新的，直接删掉，因为可能是卸载有残留导致的   

## linux常用的几个命令
    cp
    ls
    find
    sudo
    vim
    cat tail head less more
    greb
    bash连接符 && ||
    tar
    xz
    shutdown -h now
    reboot
    poweroff
    dmesg **启动日志**
    lspci　**查看pci借口**
    ifconfig　**查看网络配置**
    apt install/search/remove/purge/autoremove/autopurge  包名
    screenfetch **装逼专用**，如果出错可以尝试用 `sudo`  运行

## linux下要掌握的基本工具
    sox　音频处理软件，瑞士军刀（ps，linux下的瑞士军刀有很多）
    netcat 军刀，网络。。。还未熟悉
    convert 图像处理软件　军刀
    mplayer　最牛的视频播放软件了大概，虽然没有gui
    tr 字符处理
