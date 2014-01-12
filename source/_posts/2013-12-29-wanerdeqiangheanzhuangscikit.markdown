---
layout: post
title: "Mac OS 10.9 安装scikit--万恶的墙"
date: 2013-12-29 14:26
comments: true
categories: 
---

##前言

工作以来，接触不少机器学习相关的知识，一直听说python在这方面有许多优秀的工具，顺便自己也想学习python。经google后，决定安装scikit-learn试试。在官网上找到安装教程后，开心的发现支持brew。
[安装教程](https://gist.github.com/stared/4730202)

悲剧从此发生。。。brew需要安装的源许多都在SourceForge上，但是该网站被天朝强大的“墙”屏蔽了。没有办法一键安装。

##源码安装
我也很纳闷为什么brew的源大部分在SourceForge上，为何不用GitHub的。既然不能一键安装，那我通过源码安装总可以吧。
scikit需要numpy和scipy两个重要的组件，在scipy官网找到[安装教程](http://www.scipy.org/scipylib/building/macosx.html).

在安装scipy的提示

	/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/lipo: can't open input file:/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/lipo: can't open input file:

在查阅了一些资料后，问题貌似是由于xcode 5版本和xcode 4在工具链上有不少调整导致的，试了几种方法后未能解决。


##brew 撞墙怎么办
参考网上资料，只有自己把文件下好，放到 /Library/Caches/Homebrew/ ，但是如果文件很多，就悲剧了。

##不要轻易更新系统
在解决了brew撞墙问题后，继续尝试brew安装，依旧没有成功，提示xcode5版本太新，

	make: *** [bootstrap] Error 2
	Error: Homebrew doesn't know what compiler versions ship with your version
	of Xcode (5.0.2).
	

##未完待续
	

