---
layout: post
author: "kele"
date: "2018-05-18 22:29:13"
title: "mit 操作系统 Lab 1" 
tags: 
  - "mit operating system"
---
# mit operating system Lab 1
https://www.cnblogs.com/orlion/p/5765339.html
## part I：PC Bootstrap
第一部分主要是简单的了解实验环境、汇编语言、内存的的固定位置等基本知识，同时运行试验环境，需要注意的是mit的实验只能在32位系统上运行，尽管qemu-i368模拟的是32位机，但是你就是要用32位系统进行运行，否则就会卡住。。。      
重要的话说三遍(mit lab 1 卡住):    
** 必须用32位系统运行 **   
** 必须用32位系统运行 ** 
** 必须用32位系统运行 **  

```

+------------------+  <- 0xFFFFFFFF (4GB)
|      32-bit      |
|  memory mapped   |
|     devices      |
|                  |
/\/\/\/\/\/\/\/\/\/\

/\/\/\/\/\/\/\/\/\/\
|                  |
|      Unused      |
|                  |
+------------------+  <- depends on amount of RAM
|                  |
|                  |
| Extended Memory  |
|                  |
|                  |
+------------------+  <- 0x00100000 (1MB)
|     BIOS ROM     |
+------------------+  <- 0x000F0000 (960KB)
|  16-bit devices, |
|  expansion ROMs  |
+------------------+  <- 0x000C0000 (768KB)
|   VGA Display    |
+------------------+  <- 0x000A0000 (640KB)
|                  |
|    Low Memory    |
|                  |
+------------------+  <- 0x00000000
```

### Exercise 2: 单步运行

```
The target architecture is assumed to be i8086
[f000:fff0]    0xffff0:	ljmp   $0xf000,$0xe05b 
0x0000fff0 in ?? ()
```
可以看到系统运行的第一条指令在f000:fff0位置处，物理地址在0xffff0处，在上面的图上可以看到0xffff0是bios区域的尾部(边界是0x100000)，所以只留了16bytes,基本做不了什么，所以需要跳转到bios前面的区域来运行其他的代码,指令是 `ljmp $0xf000,$0xe05b`,跳转到 0xf000:0xe05b的地址处，这个地址存放的是BIOS的加电自检程序POST(Power-On Self-Test)，接下来的过程不必太过关心，毕竟这是操作系统的课程，参考链接在文末:     

### POST加电自检的主要流程

  首先系统运行开机之后的第一条指令，位于FFFF0到ffff4处存放的`ljmp`,跳转到POST(power-on self-test)开机自检程序,需要注意的是这里有一个冷启动和热启动的区别:只有冷启动才会开机自检，热启动不会   
  开机自检程序通过执行一些cpu运算，捕获错误或是不一致的指令开始。然后通过校验和检查BIOS的完整性，之后BIOS会创建8253计时器/定时器设备     
  之后测试视频接口
  接下来检查内存，通过全写0，全写1，或者交替写入，假如出错，电脑就会通过蜂鸣器有规则的响几声(通常可以查阅手册来进行检查)，然后关机。   
  接下来检查键盘。   
  检查BIOS Extension Roms,(感觉像是统一编址使得显示卡的内存编址映射在主存地址？？),BIOS extension roms为显示卡和硬盘接口,现在这个功能已经不太需要了   
  然后安装程序会开启硬盘接口中断，重置硬盘控制器  
  接下来建立键盘输入缓冲区,然后把并行和串行端口的默认超时值写到RAM中，然后检查有多少个串并行端口，并且把他们的I/O地址写到RAM中的数据区地址表中，这个表就位于内存中断向量表的上方.
  之后开启NMI(非可屏蔽中断位)   
  最后POST会通过蜂鸣器响一声，调用19号中断来启用BIOS ROM中的引导程序，这个引导程序会启动硬盘上的引导程序，移交控制权，之后引导操作系统启动



## part II

软盘和硬盘每512bytes叫做一个扇区，扇区是硬盘的最小传输单位，读写操作必须以一个扇区为基本单位。如果硬盘是可引导的，第一个扇区就是引导扇区，引导代码应该放在这个位置。BIOS会把引导扇区加载到内存中0x7c00-0x7dff中。BIOS会使用一个`jmp`跳转跳到0x7c00的位置，把控制权交给引导程序    
引导光盘是之后的事情了，因为光盘的一个扇区大小是2048bytes，BIOS可以加载更大的引导程序到内存里(不只是一个扇区了)。    

在实验中，boot loader 包含了一个汇编源文件 `boot/boot.S`，还有一个C源文件 `boot/main.c`:   
1. 首先boot loader 把处理器从实模式转换成为32位保护模式，因为实模式只能寻址1M的内存空间，而32位保护模式可以寻址$2^{32}$ bytes也就是4GB的内存空间
2. 然后boot loader通过直接访问IDE硬盘读取内核
```
#include <inc/mmu.h>

# 启动cpu，切换到32位保护模式，然后跳转到c语言中
# bios 从硬盘的第一个段中把代码载入到物理内存地址0x7c00中，然后开始以实模式启动 cs:0 ip:7c00(代码段寄存器和中断指针寄存器)

# 类似于c与语言中的define定义常量
.set PROT_MODE_CSEG, 0x8         # 定义内核保护模式 代码段寄存器
.set PROT_MODE_DSEG, 0x10        # 定义内核保护模式 数据段寄存器
.set CR0_PE_ON,      0x1         # protected mode enable flag 保护模式开启标志位 

.globl start
start:
  .code16                     # Assemble for 16-bit mode
  cli                         # Disable interrupts
  cld                         # String operations increment

  # Set up the important data segment registers (DS, ES, SS).
  xorw    %ax,%ax             # Segment number zero
  movw    %ax,%ds             # -> Data Segment
  movw    %ax,%es             # -> Extra Segment
  movw    %ax,%ss             # -> Stack Segment

  # Enable A20:
  #   For backwards compatibility with the earliest PCs, physical
  #   address line 20 is tied low, so that addresses higher than
  #   1MB wrap around to zero by default.  This code undoes this.
seta20.1:
  inb     $0x64,%al               # Wait for not busy
  testb   $0x2,%al
  jnz     seta20.1

  movb    $0xd1,%al               # 0xd1 -> port 0x64
  outb    %al,$0x64

seta20.2:
  inb     $0x64,%al               # Wait for not busy
  testb   $0x2,%al
  jnz     seta20.2

  movb    $0xdf,%al               # 0xdf -> port 0x60
  outb    %al,$0x60

  # Switch from real to protected mode, using a bootstrap GDT
  # and segment translation that makes virtual addresses 
  # identical to their physical addresses, so that the 
  # effective memory map does not change during the switch.
  lgdt    gdtdesc
  movl    %cr0, %eax
  orl     $CR0_PE_ON, %eax
  movl    %eax, %cr0
  
  # Jump to next instruction, but in 32-bit code segment.
  # Switches processor into 32-bit mode.
  ljmp    $PROT_MODE_CSEG, $protcseg

  .code32                     # Assemble for 32-bit mode
protcseg:
  # Set up the protected-mode data segment registers
  movw    $PROT_MODE_DSEG, %ax    # Our data segment selector
  movw    %ax, %ds                # -> DS: Data Segment
  movw    %ax, %es                # -> ES: Extra Segment
  movw    %ax, %fs                # -> FS
  movw    %ax, %gs                # -> GS
  movw    %ax, %ss                # -> SS: Stack Segment
  
  # Set up the stack pointer and call into C.
  movl    $start, %esp
  call bootmain

  # If bootmain returns (it shouldn't), loop.
spin:
  jmp spin

# Bootstrap GDT
.p2align 2                                # force 4 byte alignment
gdt:
  SEG_NULL				# null seg
  SEG(STA_X|STA_R, 0x0, 0xffffffff)	# code seg
  SEG(STA_W, 0x0, 0xffffffff)	        # data seg

gdtdesc:
  .word   0x17                            # sizeof(gdt) - 1
  .long   gdt                             # address gdt

```


<del> shanchu </del>

## 参考资料 
* [冷启动和热启动的区别](http://smilejay.com/2011/05/cold-reset-and-warm-reset/)  
* [THE BIOS, POST and the Boot Strap Loader](http://web.archive.org/web/20040809230720/http://members.iweb.net.au:80/~pstorr/pcbook/book1/post.htm)   
* [mit jos 学习笔记](http://www.cnblogs.com/fatsheep9146/category/769143.html)   
* [mit 6.828 学习笔记](https://xinqiu.me/2016/10/15/MIT-6.828-1/)
* [mit operating system lab1](https://pdos.csail.mit.edu/6.828/2017/labs/lab1/)