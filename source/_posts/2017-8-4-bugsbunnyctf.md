---
layout: post
author: kele
date: 2017-8-4
title: "坑爹的bugsbunnyctf复现"
catlog: true
tags:
  - 学习使我快乐
  - 总结
---

## Like a Boss!
题目给了一个有ascii字符组成的图案，仔细观察应该可以发现除了M和回车还有:还有.等无意义的字符，使用工具去除就可以得到答案   
wp中给的答案是`tr -d 'M.\: \n<<likeaboss.txt`，我一开始以为后面的字符串是正则表达式，后来发现并不是，只是一个匹配的规则，只要输入`tr --help`就可以查看了     
但是自己查看文档发现中':'前面的的'\'是不必要的，因为':'并不需要转义     
附上两份文档[英文版](http://www.gnu.org/software/coreutils/coreutils.html)[中文版](http://man.linuxde.net/tr)   

## primitive encryption
>Hi Hacker

>What about a time travel ? ^^
Your mission is to be very observative, sometimes a theory start from a supposition.
Decrypt this and you will get your flag :
i KTAX ZTRTRTC SuB AKXy KXlp you GiRViRF youN GlTF TRV iA'C FFAKTAwTCXTCy
To validate the challenge enter : Bugs_Bunny{your_flag_uppercase}
By Mabrouki Fakhri MrGoOD     

无法言说的痛，替换加密，字符表是用猜的...........
第一个是i,应该是不变的， kxlp>>help, youN>>your
i he...................
骗你的，233333333,突然发现可以使用[词频工具](http://quipqiup.com/)
`i HATE BANANAS CuM THEy HElp you FiNDiNG youR FlAG AND iT'S GGTHAT?ASEASy`
但是呢，有一定是错误在里面比如CUm，根据寓意可以推测成为but，还有最后的flag有一个问号，把原来的w放回去就可以了   

## UNKOWN file!!
不知道是什么文件，先用cat查看了一下，乱码(；´д｀)ゞ，于是使用xxd命令查看，
![](http://or4d8nhvk.bkt.clouddn.com/17-8-4/74210510.jpg)
妥妥的16进制，看文件头89,ascii值是gnp，翻过来是png,所以应该是一个图片的二进制翻转了，上python   
```
#coding:utf-8

input=open("C:/Users/kele/Desktop/bugsbunnyctf/UNKOWN","rb")
output=open("zz1.png","wb")

output.write(input.read()[::-1])

input.close()
output.close()
```
然后打开zz1.png就可以看到flag了。

## Give me the Flag
下载之后是一个压缩包，解压，一个flag压缩包，一个flag文件夹，压缩包有密码，猜测伪加密失败，看法flag文件夹里面除了国旗还有其他类似于二维码的图片，把他们拼接起来，组成一个qrcode，     
**敲黑板** 拼接二维码的要点，先放置四个定位的角，然后找边，因为qrcode的四周是空白的，所以找图片边是空白的尝试，知道不会出现比较窄测边为止   
![](http://or4d8nhvk.bkt.clouddn.com/17-8-4/66675645.jpg)
得到压缩包密码，解压，打开flag.txt，提示没有flag，看readme，二进制,转换ascii，得到flag

## fix me
技术太高，暂且一放

## LQI_X

## for25
下载文件16进制，转成文件，pk头部，是个压缩包
xxd工具直接转换文件，`xxd -r  hex zz.rar`，得到文件，解压，获得图片上的flag

## zero-one
zero变成0，one变成1,二进制转换ascii，ascii是个base64，在转换，得到flag

## steg10
exiftools查看直接在comment里面获得flag

## crypto-25
file 命令查看，是个txt,查看txt，发现许多ok，是类似与brainfuck的东西，[加解密网址](https://www.splitbrain.org/_static/ook/)

## crypto-20
[brainfuck](http://esoteric.sange.fi/brainfuck/impl/interp/i.html)

## scy way
看样子是个凯撒密码，但是解不出来，看wp，说是caesar box =2，解了半天，无果，最后看了wp引用的一张图片
![](http://or4d8nhvk.bkt.clouddn.com/17-8-7/16966538.jpg)  
恍然大悟，是TM的栅栏密码

## Crypto-15
```

# -*- pbqvat: hgs-8 -*-
#/hfe/ova/rai clguba
vzcbeg fgevat
# Synt : Cvht_Cvooz{D35bS_3OD0E3_4S3_O0U_T3DvS3_BU_4MM}
qrs rapbqr(fgbel, fuvsg):
  erghea ''.wbva([
            (ynzoqn p, vf_hccre: p.hccre() vs vf_hccre ryfr p)
                (
                  ("nopqrstuvwxyzabcdefghijklm"*2)[beq(pune.ybjre()) - beq('n') + fuvsg % 26],
                  pune.vfhccre()
                )
            vs pune.vfnycun() ryfr pune
            sbe pune va fgbel
        ])


qrs qrpbqr(fgbel,xrl):
    cnff


vs __anzr__ == '__znva__':
    xrl = [_LBHE_XRL_URER_]
    cevag qrpbqr("Cvht_Cvooz{D35bS_3OD0E3_4S3_O0U_T3DvS3_BU_4MM}",xrl)
```
**Cvht_Cvooz{D35bS_3OD0E3_4S3_O0U_T3DvS3_BU_4MM}** 是flag的形式，rot13加密无果，直接上js脚本,chrome的console里面不能写print，否则会调用打印命令
'''
function rot(s, i) {
            return s.replace(/[a-zA-Z]/g, function (c) {
                return String.fromCharCode((c <= 'Z' ? 90 : 122) >= (c = c.charCodeAt(0) + i) ? c : c - 26);
            });
        }
s="Cvht_Cvooz{D35bS_3OD0E3_4S3_O0U_T3DvS3_BU_4MM}"
for(var i=1;i<=25;i++){
    console.log(rot(s,i))
}
'''

    Dwvu_Dwppa{E35cT_3PE0F3_4T3_P0v_U3EwT3_Cv_4NN}
    VM2372:2 Exwv_Exqqb{F35dU_3QF0G3_4U3_Q0w_V3FxU3_Dw_4OO}
    VM2372:2 Fyxw_Fyrrc{G35eV_3RG0H3_4V3_R0x_W3GyV3_Ex_4PP}
    VM2372:2 Gzyx_Gzssd{H35fW_3SH0I3_4W3_S0y_X3HzW3_Fy_4QQ}
    VM2372:2 Hazy_Hatte{I35gX_3TI0J3_4X3_T0z_Y3IaX3_Gz_4RR}
    VM2372:2 Ibaz_Ibuuf{J35hY_3UJ0K3_4Y3_U0a_Z3JbY3_Ha_4SS}
    VM2372:2 Jcba_Jcvvg{K35iZ_3VK0L3_4Z3_V0b_A3KcZ3_Ib_4TT}
    VM2372:2 Kdcb_Kdwwh{L35jA_3WL0M3_4A3_W0c_B3LdA3_Jc_4UU}
    VM2372:2 Ledc_Lexxi{M35kB_3XM0N3_4B3_X0d_C3MeB3_Kd_4VV}
    VM2372:2 Mfed_Mfyyj{N35lC_3YN0O3_4C3_Y0e_D3NfC3_Le_4WW}
    VM2372:2 Ngfe_Ngzzk{O35mD_3ZO0P3_4D3_Z0f_E3OgD3_Mf_4XX}
    VM2372:2 Ohgf_Ohaal{P35nE_3AP0Q3_4E3_A0g_F3PhE3_Ng_4YY}
    VM2372:2 Pihg_Pibbm{Q35oF_3BQ0R3_4F3_B0h_G3QiF3_Oh_4ZZ}
    VM2372:2 Qjih_Qjccn{R35pG_3CR0S3_4G3_C0i_H3RjG3_Pi_4AA}
    VM2372:2 Rkji_Rkddo{S35qH_3DS0T3_4H3_D0j_I3SkH3_Qj_4BB}
    VM2372:2 Slkj_Sleep{T35rI_3ET0U3_4I3_E0k_J3TlI3_Rk_4CC}
    VM2372:2 Tmlk_Tmffq{U35sJ_3FU0V3_4J3_F0l_K3UmJ3_Sl_4DD}
    VM2372:2 Unml_Unggr{V35tK_3GV0W3_4K3_G0m_L3VnK3_Tm_4EE}
    VM2372:2 Vonm_Vohhs{W35uL_3HW0X3_4L3_H0n_M3WoL3_Un_4FF}
    VM2372:2 Wpon_Wpiit{X35vM_3IX0Y3_4M3_I0o_N3XpM3_Vo_4GG}
    VM2372:2 Xqpo_Xqjju{Y35wN_3JY0Z3_4N3_J0p_O3YqN3_Wp_4HH}
    VM2372:2 Yrqp_Yrkkv{Z35xO_3KZ0A3_4O3_K0q_P3ZrO3_Xq_4II}
    VM2372:2 Zsrq_Zsllw{A35yP_3LA0B3_4P3_L0r_Q3AsP3_Yr_4JJ}
    VM2372:2 Atsr_Atmmx{B35zQ_3MB0C3_4Q3_M0s_R3BtQ3_Zs_4KK}
    VM2372:2 Buts_Bunny{C35aR_3NC0D3_4R3_N0t_S3CuR3_At_4LL}    get the flag


## web180
nosqlinjection mongodb  json $ne  双注入？？？？？待续

##  Old php vuln 35 pt
万能密码尝试无效，使用数组绕过............

## lost data
http://www.garykessler.net/library/file_sigs.html
