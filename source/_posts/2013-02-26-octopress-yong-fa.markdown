---
layout: post
title: "Octopress 用法"
date: 2013-02-26 14:59
comments: true
categories: Octopress 
---
第一次使用Octopress，如此简洁的感觉看起来很爽，下面是发文章的基本用法

#### 新文章
	rake new_post["title"]

#### 编辑文章内容
用vim即可或者其他编辑器，暂时还不会。
默认内容：
	---
	layout: post
	title: "Octopress 用法"
	date: 2013-02-26 14:59
	comments: true
	categories: Octopress 
	---
头两个"---"是文章属性，意义很直接。
*categories*是文章分类，记得*:*后面要加个*空格*，如果想设置多个分类可以如下：
	categoriest: [公务猿, 苦逼]
或
	categoriest:
	- 公务猿
	- 苦逼
默认内容中第二个"---"下面就是文章内容区域，需要使用*markdown*语法书写，这个需要自己去细查。
记住文章内容最后还需要加一个
	---
#### 生成静态内容
	rake generate

#### 发表文章
	rake deploy
---
#### 引用
*	[Octopress Usages](http://cofthew7.github.com/blog/2012/06/23/octopress-usages/)
