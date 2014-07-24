---
layout: post
title: "curl 用法"
date: 2013-03-19 16:18
comments: true
categories: Linux 
---
*curl*可以作为下载命令使用，特此记录。
### 下载文件
下面为直接下载文件，保存为原文件名：
	curl -O  http://mif.polimercolor.ru/mifsoft/MDict.zip

下面为修改文件名：
	curl -O MDict_ver.zip  http://mif.polimercolor.ru/mifsoft/MDict.zip

### 断点续传
下面为断点续传的方法, 使用*-C*选项，利用*-C -*选项并指出已经部分下载的文件名，curl将自动下载剩余的部分:
	curl -C - -o Smultron-2.2.6.dmg http://jaist.dl.sourceforge.net/Smultron-2.2.6.dmg

### 使用代理服务器
	curl -x 10.1.27.10：1022 ftp://ftp.funet.fi/README
以上为不需要账号密码的代理服务器下载方法，下面为提供账号密码：
	curl -U user:password -x 10.1.27.10:1022 ftp://ftp.funet.fi/README
### 更多功能
使用 man curl，自行查看帮助。

#### 引用
[使用cURL下载文件](http://yyq123.blogspot.com/2009/09/curl.html)	

---
