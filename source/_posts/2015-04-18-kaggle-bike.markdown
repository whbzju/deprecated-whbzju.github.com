---
layout: post
title: "Kaggle入门总结"
date: 2015-04-18 20:38
comments: true
categories: machine learning
---



在知乎上看过一个答案，大意是有个地方叫kaggle，推荐搞机器学习的同学多上去撸一撸，实践出真知。同时还建议先把101系列的题目撸完，再选个感兴趣的比赛做。该答案详情见参考文献。

## 工具
有工程背景的同学，建议python，拥有不输给R的生态。主要用到以下工具：
	
	ipython notebook + pandas + sklearn
	
在面对特别大的数据集，使用了公司的spark。

* ipython notebook，神器，请参考我的另一篇blog [Ipython Notebook对机器学习工程师的价值](http://www.wujiame.com/blog/2014/11/23/ipython-notebook-bring-to-me/)
* pandas: 从工程过来的同学，首先请放弃循环之类的代码实现方式，拥抱dataframe。
* sklearn：在github上非常活跃的项目，请多读官方文档。
* spark：一般kaggle上比赛的数据量级是没有必要用它，但是最近有个比赛train的数据上百g了，所以试了下它。

## 比赛选择

首先，请从101系列中选几个做做，该系列一般有详细的教程，熟悉kaggle。接着选几个正在进行的比赛练手。一开始别贪心，注意下数据集的大小，当数据集大于几个g后，工程相关的工作会增加很多，同时对单机的性能有一定的要求，不利于初学者。但是，数据量大更符合真实的情况，比如做过一个ctr预估的比赛，无论是特征工程和模型训练都要更小心谨慎，每次试错的成本很高，随便训练一个模型都需要3-4小时，相应的这个比赛让我意思到sample的重要性，以及一个非常重要的特征处理方法[featrue hashing](http://www.wujiame.com/blog/2015/02/10/feature-hashing/).

本文将重点总结我在做自行车出租数量预测这个比赛的情况。该比赛介绍如下：

> You are provided hourly rental data spanning two years. For this competition, the training set is comprised of the first 19 days of each month, while the test set is the 20th to the end of the month. You must predict the total count of bikes rented during each hour covered by the test set, using only information available prior to the rental period.

该比赛的好处是符合kaggle的特点。

1. feature engineering是第一要素。
2. ensemble大法好。bagging、averaging一起上，比如到最后我喜欢用random forest + gbdt组合再搞一把，总能给我惊喜。
3. 时刻小心过拟合。

## 特征工程

1. 异常值处理，nan，outlier等。hist和box是两个常用的图形工具。
2. 数据分布倾斜。 log变化、正负样本重新抽样等。
3. 特征交叉组合 
4. pca或random forest的特征重要性选择。
5. 特征之间的相关系数。
6. 特征onehotencoding

## 模型选择

1. Logistic regression
2. Random Forest 
3. gbdt和gbrt
4. Factorization Machines

LR对特征要求更高，比如很多categorical的特征要作binary编码。我个人倾斜于RF和gbdt，都是属于ensemble思想下的算法。具体RF算法可以参考以前写的一篇blog：[random forest](http://www.wujiame.com/blog/2015/02/16/random-forest/)

Factorization Machines这个算法好多人用它做ctr预估，后续可以研究下。

## 代码分享

具体代码实现请看我的notebook：
[kaggle上的自行车出租数量预测](http://nbviewer.ipython.org/gist/whbzju/ff06fce9fd738dcf8096)



## 参考文献

[1] 知乎答案[Kaggle如何入门](http://www.zhihu.com/question/23987009)

[2] 知乎答案[参加kaggle竞赛是怎样一种体验？](http://www.zhihu.com/question/24533374)

