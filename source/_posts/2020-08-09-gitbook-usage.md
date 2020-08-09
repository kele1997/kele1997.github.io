---
title: gitbook 使用体验
date: 2020-08-09 09:49:27
tags:
---

# Gitbook 使用体验

最近要写 openstack 组件的部署文档，因为有很多的组件，决定把文档拆分来写。最后可能要发布成文件，提供为他人浏览，所以需要一种可以导出到 pdf 的工具来编写文档。   
尝试了几种工具，最终敲定了 gitbook 


常见的 markdown 编写文档工具   
1. docsify 只能导出html 
2. mdbook rust 编写，单二进制，有一个导出到epub的插件。以及所有页面可以通过浏览器导出到单pdf文件（没有目录）
3. sphinx  好像不太方便
4. gitbook 支持导出 pdf(需要安装 calibre) ，可以导出 html(本地浏览需要修改 js), 不再维护，但是可以使用   


## Gitbook 安装
需要提前安装 `node.js`, `npm` 工具   
国内源很慢，可以考虑更换npm淘宝源(详情可以百度)   

PS: gitbook 似乎支持到 node 10.21.0 的版本
安装 gitbook:   `npm install -g gitbook`   
安装 gitbook-cli: `npm install -g gitbook-cli`  

## 生成 gitbook 目录 
```
gitbook init demo
```

目录中的 `SUMMARY.md` 是文档的层次，以及生成的目录     
目录中的其他 `README.md` 是必须文档，需要概括文档的内容   

具体使用可以参考网上许多的文档  
https://chrisniael.gitbooks.io/gitbook-documentation/content/   


需要注意的是， gitbook 3.x 版本的目录都是 1.x ,似乎是 gitbook 3.x 的bug，据说要 4.x 修复，但是停止维护了，不会修复了。   
但是 gitbook 3.x 有一个特性是 part ，可以在 SUMMARY.md 中加入 `---` 就可以分段   

```
# Summary

- [1. Chapter 1](./chapter_1.md)
---
- [2. Chapter 2](./chapter_2.md)

```


## 导出 pdf 
最新版的 gitbook 导出 pdf ，需要使用 `calibre`, 可以参考 https://calibre-ebook.com/download_linux 进行安装
之后，在 gitbook 项目目录下，执行 gitbook pdf ，即可得到 pdf

## 导出 html 
执行 gitbook build ，可以生成静态 html      
静态 html，本地浏览跳转不正常，需要修改 gitbook/theme.js ，搜索 `if(m) for` ，改为 `if(false) for`  ，就可以正常跳转了   



## 封面插件
自动封面插件 autocover  

背景颜色有 bug ,总是黑色，需要修改 svg 文件  
https://github.com/GitbookIO/plugin-autocover/issues/20   


## 总结 
gitbook 太烂了。。。。   
gitbook 下载插件速度慢， 许多插件也是停止维护，本地也停止维护    
尽可能使用 mdbook ，单二进制程序，虽然导出的 pdf 没有目录，但是足够简单，留出充足的时间来写文档本身    

(mdbook 似乎有一个 mdbook-toc的插件, 也可以考虑使用 calibre 手动将 html 转换为 pdf(也很蛋疼))