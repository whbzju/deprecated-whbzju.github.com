---
layout: post

title: "ipython notebook对机器学习工程师的价值"

date: 2014-11-23 16:17

comments: true
categories: machine learning
---


**关键词**：代码、数据、文档合一。

-------

如果选一个关键词来描述机器学习工程师的工作，不断试错是我心中的number one。相对于软件工程师来讲，有大量琐碎的dirty需要做，通常会占据到80%左右的时间。一个好的工具能够极大的提高效率。

总结需求如下：

1. 可交互式的环境：比如预处理数据，有的时候数据比较大，比较耗时，希望能处理一次后就放在内存里面使用。
2. 文档化，记录工作流。数据挖掘会有非常多的idea要去尝试，实现这些idea的代码会有微小的差异，需要一个工具能够统一追踪管理他们。且不同的实验会有不同的结果，整理这些结果形成文档太费时间，希望能够做完实验就生成文档。
3. 经常会有一些片段代码要写，写在文件里有太零碎，写在交互式的shell里面有很难回溯，需要一个交互式和文档结合的工具。
4. 支持可视化工具，兼容python画图

## 神奇的ipython notebook

### 安装环境
非常简单，推荐：Anaconda, [官网](https://store.continuum.io/cshop/anaconda/)

### ipython notebook入门
还是[官网](http://ipython.org/notebook.html), 一开始不适应的同学，多看几个example吧。

### 分享你的ipython notebook
一键分享：[A simple way to share Jupyter Notebooks](http://nbviewer.ipython.org/)


## 最后
附一张我的在kaggle上用ipython notebook做的一个入门题照：
![剧照](http://wujiarawsrc.qiniudn.com/sklearn-kaggle.png)
