---
layout: post
title: "ubuntu 12.04安装bochs 2.3.5"
date: 2013-02-04 23:37
comments: true
categories: TryOS 
---

一直对操作系统本身抱有一定的崇拜心理，可是长久以来都没有好好专研下这本书。趁着工作之前的最后一个寒假，希望能顺利完成这本书，对操作系统的认识有一个新的认识。
> 工欲善其事，必先利其器
首先，我们先要配置出书本中模拟实验的环境，即虚拟机bochs的配置。众所周知，bochs的安装包版本是不带有调试功能的，只有通过编译bochs源码，才能启用debug模式。

##bochs 2.3.5源码编译
网上编译bochs的资料非常多，基本的问题都有解决方案，我重点讲不常见的问题。
**基本安装步骤**
	tar vxzf bochs-2.3.5.tar.gz
	cd bochs-2.3.5
	sudo ./configure --enable-debugger --enable-disasm
	sudo make
	sudo make install
bochs是c++实现的，因此其依赖环境会有build-esstianl g++ devlib等，需事先安装环境
	sudo apt-get install build-essential
	sudo apt-get install xorg-dev //GUI界面
	sudo apt-get install bison

在执行./configure时，出现apt-get orgx-dev后依旧出现仍然提示*ERROR: X windows gui was selected, but X windows libraries were not found*
<!--more-->
采用解决办法：
> 只要编译的时候连接了 -lX11这个库就可以了，所以可以让configure阶段出错的地方不退出，并且在make的时候link X11这个库，编辑configure, 将退出的地方注释掉
    echo ERROR: X windows gui was selected, but X windows libraries were not found.
        #exit 1

	configure命令后加 LDFLAGS="-L/usr/lib/i386-linux-gnu -lX该问题不能用--with-nogui解决，否则无法输出hello os，因为需要使用gui

**make**之前需要修改一份文件bx\_debug/symbol.cc
	在97行之后加入代码如下,
	using namespace std;

	#ifdef __GNUC__ //修改
	using namespace __gnu_cxx; //修改
	#endif //修改

	struct symbol_entry_t
keymap若提示找不到，注释掉即可。
##bochsrc
bochsrc是bochs启动时读取配置的文件，其中关键的是romimage和vgaromimage的路径设置。关于rom，install vagbios后，/usr/share/bochs路径存在，romimage路径在ubuntu下：/usr/local/share/bochs，修改下即可
##制作引导盘
用bximage制作软盘映像
`bximage
按提示制作
将引导扇区写入软盘
`dd if=boot.bin of=a.img bs=512 count=1 conv=notrunc
##启动bochs
终端中输入bochs，按提示输入，在debug模式下，需要输入c让程序运行。若一切顺利，能看到屏幕上输出hello os的字符。
##关于64位机子的问题
在configure时，enable-long-phy-address不存在，无法顺利支持64位寻址，需进一步研究确认。

##诡异问题
依旧还有unknow VEG error，不知道怎么解决。

