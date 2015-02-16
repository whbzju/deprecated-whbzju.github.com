---
layout: post
title: "random forest"
date: 2015-02-16 15:09
comments: true
categories: machine learning
---

## 概述

### 知识背景要求

本文要求读者对机器学习中的一些基本概念有一定了解，比如特征，交叉验证，generation等概念。随机森林基于决策树模型，读者事先最好对决策树有一定的了解，若完全不了解，请将文中的tree抽象成能告诉你对错的一个black box，则不会影响理解。

### 目录

1. 基本思想
2. 理论保证
3. 实践中常用的特性
4. 实践效果验证
5. 需要重点注意的
6. 参考

## 基本思想

### Ensemble method

ensemble是当前主流机器学习领域一个非常流行的概念。引用sklearn的文档：

> The goal of ensemble methods is to combine the predictions of several base estimators built with a given learning algorithm in order to improve generalizability / robustness over a single estimator.

其又分为两大类：averaging和boosting，分别以Random Forest和AdaBoost算法为代表。

### Random Forest

引用wiki的定义：

>Random forests are an ensemble learning method for classification, regression and other tasks, that operate by constructing a multitude of decision trees at training time and outputting the class that is the mode of the classes (classification) or mean prediction (regression) of the individual trees. 

直白来讲，随机森林就是将一堆决策树聚在一起，形成一个森林，让每个决策树对测试目标投票，最后对结果求平均，得到最终结果。举个例子：今天你要预测中国队能不能进入世界杯，你找了一百个朋友，让他们每个人给个预测，最后对所有人的预测结果求平均，得到最后中国队能不能出线的结论。直观来理解，这个模型最终结果受到两个因素影响，这一百个朋友中每个人对足球的了解程度和这些人之间的差异程度。

换个正式点的描述：首先，我们假设你了解如果构造一个决策树，随机森林构造了很多个决策树，对于一个数据集N，每次随机抽取n个数据和m个特征，构造一个完全生长的树（不用剪枝），将数据放回重复上述过程，直到生成M棵树。其中有放回的抽样称为bootstrap，是非常重要的概念，下文中让随机森林不用交叉验证的out-of-bag理论基于此。在最初的那篇论文，提到该模型的error受两个因素影响：
	
	* 每个树之间的correction。correction增加，error增加。
	* 单独一颗树的strength（不知道怎么翻译，能力？）。strength增加，error下降。

随机选取m个特征是个关键的步骤，假设总特征有M个，m越小，correction和strength越小。m越大，correction和strength越大。一个合理的m很重要。

## 理论保证

写公式好累，大家去看参考文献吧。


## 常用特性

* oob 
* feature importance
* 聚类

### The out-of-bag (oob) error estimate

前面提到过boostrap，这个抽样机制从理论上决定了每次抽样有近三分之一的数据不会被抽到，即可以直接拿来做为测试集，使random forest免去cross validation的过程，节省了时间，称为oob。

### feature importance

random forest是个black box，feature importance特性有助于模型的可解释性。简单考虑下，就算在解释性很强的决策树模型中，如果树过于庞大，人类也很难解释它做出的结果。随机森林通常会有上百棵树组成，更加难以解释。好在我们可以找到那些特征是更加重要的，从而辅助我们解释模型。更加重要的是可以剔除一些不重要的特征，降低杂讯。比起pca降维后的结果，更具有人类的可理解性。

feature importance有好几种方案实现，最常用的是基于一个思想：如果该特征非常的重要，那么稍微改变一点它的值，就会对模型造成很大的影响。再偷个懒，自己造数据太麻烦，可以直接在数据集对该维度的特征数据进行打乱，重新训练测试，打乱前的准确率减去打乱后的准确率就是该特征的重要度。该方法又叫permute。

### Unsupervised learning 聚类

有点出乎意料，原来随机森林还可以做聚类。总所周知，聚类算法的关键是相识度计算，而随机森林能很好的做相识度计算。首先让我们了解下Proximities：

> The proximities originally formed a NxN matrix. After a tree is grown, put all of the data, both training and oob, down the tree. If cases k and n are in the same terminal node increase their proximity by one. At the end, normalize the proximities by dividing by the number of trees.

大致的意思是说，在随机森林模型生成时，统计case k和n在不在同一个叶子节点中，在就加1，最后在安装树的个数归一化。得到case之间的相似度。

## 实践效果验证

在工作中，随机森林被我用来做的最多的事情还是feature importance，验证自己的特征工程效果。另外，由于它的鲁棒性特别好，模型效果在各大模型中排中等，而且对数据预处理要求低，我经常会优先选择用它跑个benchmark。

业余时间还会玩玩kaggle比赛，它就用的比较多，其实gbdt应该更适合比赛，但我个人还是偏好RF。下面放个kaggle的比赛总结，关于如何用它：[Getting Started with Random Forests: Titanic Competition](https://www.kaggle.com/c/titanic-gettingStarted/details/getting-started-with-random-forests)


## 需要重点注意的

以sklearn的RandomForestClassifier为例，有几个参数需要调下。

1. n_estimators。决定模型有几颗树，我在做Click-Through Rate Prediction比赛时，将它从default 10改到100，模型效果提升了很多。
2. criterion。默认有gini和entropy可选，到底哪个好现在还是有争论的。
3. max_features，这个和n_estimators会一起影响整体结果，上文已经提到，可以参数换几个参数试试。

## 下一步计划

研究下gradient boosting decision tree

## 参考

[1 wiki RF](http://en.wikipedia.org/wiki/Random_forest)

[2 Random Forests Leo Breiman and Adele Cutler](http://www.stat.berkeley.edu/~breiman/RandomForests/cc_home.htm#inter)

[3 Ensemble methods](http://scikit-learn.org/stable/modules/ensemble.html)














 