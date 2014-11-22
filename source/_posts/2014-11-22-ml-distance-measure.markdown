---
layout: post
title: "聚类算法中常见的距离计算方法"
date: 2014-11-22 22:01
comments: true
categories: machine learning
---

## 概述
在面对聚类问题时，选择何种距离计算方法求相似度是一个basic question。文献[1]中提到了N多计算方法，从大类来看有以下几种：

* $L_p$ Minkowski家族
* $ L_1 $ 家族
* Intersection 家族
* Inner Product家族
* etl
简单算一下大概有40+个计算方法，其中有好多没有听过。好在工业界一般只涉及到几个，本文将按自己理解大致介绍下这些方法及应用情况。

## 距离的类型和尺度
类型：

* 二进制（binary）
* 离散值（Discrete）
* 连续值(Continuous)

尺度：

* 定性：比如同义：red、green、black，比如顺序：高、中、低
* 定量：
    a) interval
    b) ratio
距离的类型和尺度非常重要，影响后续聚类算法的选择。

## 距离计算方法定义
严谨的定义参考[4]，通俗来讲，在一个空间内，距离计算方法满足以下4个公理。

1. $d(x,y) ≥ 0$
2. $ d(x,y)=0$ if $x=y$
3. $ d(x, y) = d(y, x)$ (distance is symmetric)
4. $d(x, y) ≤ d(x, z) + d(z, y)$ (the triangle inequality).

在欧式空间，第四个公理可以直观理解为两点之间距离最短。在其他情况需要一些证明才能推导。

## 常见的距离计算方法

### Lr norm
在n维空间，其计算公式如下：

$d(x,y)=(\sum_{k=1}^{n} \|x_k-y_k\| ^r)^{1/r}$


* 欧式距离
当r=2，这就是我们熟悉的欧式距离，其聚类形状在二维空间是一个圆。归属于$L_2$ norm

* 曼哈顿距离
r=1，归属于$L_1$ norm，其名字的来源与该距离计算过程有关。该距离类似在x和y的每个维度上沿grid line上travel，类似曼哈顿的街道。

* $L_∞$ norm
当r不断变大，该式中只有max $|x_i-y_i|$项起作用，故又称L_max norm

### Jaccard Distance
类似于相识度，x和y每个维度上相同的值的个数/总的维度。

The comparison of two binary vectors, p and q, leads to four quantities: 

* M01 = the number of positions where p was 0 and q was 1 
* M10 = the number of positions where p was 1 and q was 0 
* M00 = the number of positions where p was 0 and q was 0 
* M11 = the number of positions where p was 1 and q was 1 

The simplest similarity coefficient is the simple matching coefficient 

$ J = (M11) / (M01 + M10 + M11) $ 

如：

* a = 1 0 0 0 0 0 0 0 0 0
* b = 0 0 0 0 0 0 1 0 0 1
J = 0

### cosine Distance
即余弦公式。求x和y两个特征向量的夹角。
cos( d1, d2 ) = (d1 • d2) / ||d1|| ||d2|| 

### Edit Distance
一般适用于String point。通过不断的删除和添加单个字符来计算两个string之间的距离。

### Hamming Distance
pass

## 参考

* [1] Comprehensive survey on distance/similarity measures between probability density functions. SH Cha
City, 2007 - csis.pace.edu
* [2] An Introduction to Cluster Analysis for Data Mining
* [3] Unsupervised and Semi-supervised Clustering:
a Brief Survey
* [4] Mining of Massive Datasets - chapter 3