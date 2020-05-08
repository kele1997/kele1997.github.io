---
layout: post
author: kele
title: "linux fstab的有关问题"
tags:
  - linux
  - 总结
date: 2017-9-11
---
# linux mint fstab的问题
* mount 命令是用来挂载磁盘的;umount 命令使用来卸载磁盘的　　　
* 把一个磁盘挂在到一个文件夹之后，这个文件夹的内容就变成了磁盘的内容，而原来的内容就会无法访问了　　　
* linux Mint　默认把磁盘挂在到`/media/kele/`目录下面，访问起来特别麻烦，所以我打算把磁盘挂在到$HOME路径下面（$HOME指的是/home/user／ 目录）

`sudo fdisk -l` 查看磁盘设备
`blkid` 查询磁盘uuid
`sudo vim /etc/fstab`
添加一行
`/dev/sdb7 /home/kele/hapi/ ntfs 0 0` **注意hapi是我手动新建的文件夹,不能直接挂载到/home/kele下面，否则开机找不到用户目录，轻则系统变为初始状态，重则无法开机，但是加入你改错了，只要改回去就可以了，[方法](#method)**    
之后开机系统就会自动挂在目录到用户目录下面了


## hibernate找不到了       
linux mint 有一个叫做suspend to disk(实为hibernate) 的功能,但是今天突然找不到了，于是就很尴尬,Google得知hibernate功能需要足够大的swap分区,于是使用 `free -h `查看swap分区，突然发现swap分区大小为0，仔细思考，发现是修改fstab的时候不小心，删除了自动挂载的swap分区，于是重新添加就可以了
`／dev/sdb5 swap swap default 0 0`
然后就可以看到重逢的suspend to disk 了


  
### <a id=method>方法就是 </a>
**修改fstab提示fstab是只读文件**
进入linux　的高级选项菜单，使用root终端，修改/etc/fstab，当然可能报错，可能会提提示fstab是只读文件，这个时候只需要重新挂载根目录即可`sudo mount / -o remount `，然后就可以修改了
