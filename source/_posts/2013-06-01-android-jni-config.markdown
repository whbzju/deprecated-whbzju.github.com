---
layout: post
title: "Android JNI MAC OS环境配置"
date: 2013-06-01 23:50
comments: true
categories: JNI
---

##前言---JNI技术简介
JNI是Java Native Interface的缩写，即“Java本地调用”，它是Java世界和Native世界的中介桥梁。其中Native世界一般指C/C++的世界。众所周知，Java是一种跨平台的语言，即Java编写的应用与平台无关。但是，计算机的世界在Java出现之前已经发展了很久，有很多优秀健壮的代码可以复用。比如Linux下的好多驱动模块、文件系统等。Java若去重新实现这些功能，未免费力不讨好，落下重复造轮子的名声。通过JNI技术，使用这些Native的模块，便成了一个折中的办法。同时，Java的世界依靠虚拟机构建，而虚拟机是native语言实现，并且虚拟机运行在具体的平台上，所以虚拟机本身是无法做到平台无关。通过JNI技术，可能做到在Java层的平台无关，即在Java层，底层的细节完全被屏蔽掉了。综合来讲，JNI技术一直支撑这Java世界，只不过我们平时接触的较少。

在Android的世界里，不允许纯C/C++的程序出现，但是它支持JNI，通过JNI来实现java和C/C++的交互。因此，JNI对于需要接触到Android源码、底层驱动、图形图像等领域的开发者来讲异常重要。

在Android中，Native语言实现的代码最终要编译成*.so动态库的方式，供java层调用，目前有两种途径实现。

##两种编译环境
* 源码编译环境：Android平台提供基于Make的编译环境，为App正确的编写Android.mk即可使用该编译环境，该环境需要通过git从Android的官方的源码平台获取源码并编译，得到环境。具体见：<http://source.android.com/index.html>
* 基于Android NDK的编译环境:NDK的全称叫做Native Development Kit。是google提供给我们用于本地编译JNI的工具。事实上，NDK和源码编译环境一样，都是使用Android的编译系统，通过Android.mk来控制编译。本文重点介绍这种方式。


##NDK编译环境
在Mac下，配置NDK的环境十分简便，你只需要去[官网](http://developer.android.com/tools/sdk/ndk/index.html)下载ndk包，前提是你已经安装好ndk需要的工具，一般你如果安装过xcode，基本的环境都会有。解压缩到任意一个目录下，把该目录加到你的PATH中即可。比如我的：

在~/.bash_profile中把路径加入PATH，如果没有，可以创建一个.bash_profile，在最后加入下面语句。

```
export PATH=$PATH:/Users/youpath/android-ndk-r8e
```
重启bash，即可使用ndk-build

``` 
HaibotekiMacBook-Air:jni haibowu$ source ~/.bash_profile
HaibotekiMacBook-Air:jni haibowu$ ndk-build

```

##运行Hello-jni
ndk包解压缩之后，自带一些jni的例子，下面我们就来编译运行下hello-jni，感觉下jni的世界。该demo的路径在ndk安装路径的sample目录下。进入该路径，执行下列命令：

```
HaibotekiMacBook-Air:jni haibowu$ ndk-build
Gdbserver      : [arm-linux-androideabi-4.6] libs/armeabi/gdbserver
Gdbsetup       : libs/armeabi/gdb.setup
Compile thumb  : hello-jni <= hello-jni.c
SharedLibrary  : libhello-jni.so
Install        : libhello-jni.so => libs/armeabi/libhello-jni.so
```
当系统提示生成*.so文件时，即代表编译成功。可以通过eclipse或者intellij idea导入该工程，运行查看效果。

##其他
* 如果你有android源码编译环境，你可以通过编写android.mk来编译app
* 如果你是在window下使用ndk，你需要安装cygwin，来模拟linux的环境，才能把ndk安装成功，其思想是一直的。参考：<http://www.cnblogs.com/luxiaofeng54/archive/2011/08/13/2136982.html>
* ndk是一个开发工具包，你也可以查看它的源码、进行编译，具体参考：<http://glandium.org/blog/?p=2146>

##后续
接下来，我会写一篇介绍Jni的blog，希望能写的浅显易懂。
