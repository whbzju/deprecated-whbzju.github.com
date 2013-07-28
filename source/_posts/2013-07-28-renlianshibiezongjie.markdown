---
layout: post
title: "人脸识别实践总结"
date: 2013-07-28 10:06
comments: true
categories: 
---
#概述
前段时间接触了一阵子人脸识别，只能说是初窥门道，在这里做个总结。本文不涉及具体的算法原理，因为我都是参考别人的资料，只认为无法写的更好，在这边做个归纳总结。
现需求如下：***从摄像头视频中识别出你自己或朋友的人脸***

要实现这个需求，大体分为两个步骤，分别为人脸检测和人脸识别，即先要从摄像头中识别出人脸，即人脸检测，其次对检测到的人脸进行识别，即人脸识别。本文基于Opencv的人脸算法实现。

##人脸检测流程


* 选取特征（本文采用Haar-like特征）
* 选取分类器算法，训练人脸分类器（本文采用Adaboost级联分类器）
* 对图像进行人脸检测


##人脸识别流程

* 选取人脸识别算法（本文包括PCA、FDA和LBP）
* 训练识别模型
* 对目标进行识别 

#Opencv相关资料介绍
opencv在2.4后引入了人脸识别相关模块，原来只有人脸检测部分。在Opencv官网，有较详细的介绍，看
[!目录](http://docs.opencv.org/trunk/modules/contrib/doc/facerec/)，在该目录中重点要看这篇[!Face Recognition with OpenCV](http://docs.opencv.org/modules/contrib/doc/facerec/facerec_tutorial.html).

这应该是一个德国人写的，在教程中他提到了3个算法：

* EigenFaces
* FisherFace
* Local Binary Patterns Histograms

前面两个算法都是利用子空间的原理，有一定的相似性，分别以PCA和LDA为基础。后者以特征选取为主，做法思路都不大一样，建议分开看。该教程中对算法的描述过于简洁，不适合初学者看，建议寻找相关资料进一步阅读。
##PCA-主成分分析法
PCA在很多地方都有应用，是一个十分简单有效的方法。其思想概括起来即降维，它认为原始数据中包含了大量的噪音和冗余，通过协方差矩阵的对角化可以得到一个子空间，该子空间的维度大大降低，却神奇的保留了原始数据中的显著特征。

该算法的具体原理可参考斯坦福大学的公开课，Andrew.Ng的机器学习课程，里面有一章节专门讲pca，若觉得看视频太慢，可以直接看讲义，讲的很清楚。国内有几个博客作者对它进行了翻译，推荐：

[!主成分分析（Principal components analysis）-最大方差解释](http://www.cnblogs.com/jerrylead/archive/2011/04/18/2020209.html)

[!机器学习中的数学(4)-线性判别分析（LDA）, 主成分分析(PCA)](http://www.cnblogs.com/LeftNotEasy/archive/2011/01/08/lda-and-pca-machine-learning.html)

该算法涉及较多的线性代数知识，忘掉的同学建议复习下相关内容。

##LDA-线性判别分析
fisherface的FDA是在LDA基础之上的一种算法。关于线性判别的思想如下：它认为在PCA中，PCA把数据作为一个整体来看，即数据源中所有的显著特征都会被保留下来，如果一个人的脸在强光下和弱光下，pca生成的子空间有显著的差异，而他们却是同一张脸。LDA的思想是寻找一个分割平面（在二维中即直线），来区分两种不同类别的数据，既能够区分两个不同的人脸，进行归类。因此，它的目标就是怎么要找到这个平面，达到最好的区分效果。

同样，该算法的具体原理还是推荐Andrew.Ng的机器学习公开课。国内也有相关介绍，但是他们的数学推导让我不满意。

[!线性判别分析（Linear Discriminant Analysis）（一）](http://www.cnblogs.com/jerrylead/archive/2011/04/21/2024384.html)
[!线性判别分析（Linear Discriminant Analysis）（二）](http://www.cnblogs.com/jerrylead/archive/2011/04/21/2024389.html)

##LBPH
该算法较上面二者容易理解，这里不做详细介绍，大家自己查找相关资料即可。

##Demo
上面教程中提到几个算法opencv中都有例子实现，当然要做2.4以上。教程讲了demo的位置和具体的使用。
所有的Demo需要一个人脸库，教程中提供了几个，可以下载。下载下来的人脸库需要预处理，即打上标签，作者提供了python脚步，大家可以使用。

有个demo值得关注，它实现了我们的需求，它有个专门的教程：[!Face Recognition in Videos with OpenCV](http://docs.opencv.org/trunk/modules/contrib/doc/facerec/tutorial/facerec_video_recognition.html). 不过要想识别自己的脸，必须将自己的脸裁剪下来保存到人脸库中进行训练。

我不想自己拍照片去裁剪，我的做法是利用demo中的人脸检测算法，将我的人脸检测到，然后保存成灰度图，放到人脸库中。PS：这里有个问题，opencv自带的人脸检测分类器有可能会误捡，会把空白的墙壁当做人脸。我的做法是，在视频中指定一个矩形框，在这个矩形框中进行人脸检测，这样可以大大降低误捡率。实际操作中可以调整位置，让自己的人脸出现在矩形框中。

#Haar-like特征Adaboost级联分类器
完成了人脸识别的Demo验证，大家一定很好奇人脸检测是怎么实现的。opencv里面自带的检测算法原至两篇论文：
P. Viola and M. Jones.  Rapid object detection using a boosted cascade of simple features.
R. Lienhart and J. Maydt.  An Extended Set of Haar-like Features for Rapid Object Detection.


##整体感觉--如何训练自己的分类器
大家可以参考这篇[!教程](http://wenku.baidu.com/view/39419a3567ec102de2bd891b.html)，利用opencv自带的例子训练一个分类器感觉感觉。
训练分类器会遇到很多问题，人脸样本和非人类样本的比例有较高的要求，stage越高越难训练，训练时间也会随之快速增长，而且效果还不能保证，博主训练出来的分类器和opencv自带的分类器效果是没法比啊！

##算法原理
我不打算这这里描述它的原理，首先是这方面资料已经很多，我无法做到写的更好，其次是展开后篇幅太长，内容太多，我不想写了，哈哈。
若想较好的理解，直接看上面提到的两篇论文。若英文水平不行，可以看北大有个学生写的毕业论文：基于 AdaBoost 算法的人脸检测，作者：赵楠，还不错。

Haar-like特征需要理解积分图的概念，Adaboost包括弱分类器、强分类器和级联分类器。其中级联分类器较比较麻烦。

#总结
Opencv是个好东西，有个直接能运行的demo，比光秃秃的理论好多了，依靠它搭个自娱自乐的小工程没问题。人脸识别的水很深，本文提到的算法是opencv里面就有的，还有很多算法待各位自己有兴趣去研究。




