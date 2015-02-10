---
layout: post
title: "机器学习基石课程总结"
date: 2014-12-28 11:07
comments: true
categories: machine learning
---
课程一开始，提了四个topic，what every machine learning user should know

	* when can ml learn
	* why can ml learn
	* how can ml learn
	* how can ml learn better

## When can ml learn
首先，机器学习针对的场景，通过*A*对*D*和*H*学习一个g，用来描述最终的目标f，而这个事情无法简单的用规则搞定。其次，澄清各类细分ml场景的定义：

	* 监督式
	* 非监督式
	* 增强学习
	* 推进系统
	* Activity学习，通过asking来学习
	* Streaming
	
## why can ml learn
	* shatter的概念
	* break point的概念
	* generation问题
	* VC维的概念
	
## how can ml learn
讲了一些基本的linear方法，比如logistic regression，顺便提了下nonlinear的问题，通过transform将nonlinear映射到linear可分的空间，有点类似核函数，需要进一步确认。

## how can ml learn better
	* overfiting
	* regularition，这块数学不错。从拉格朗日的constraint说起，到L1和L2的直观意义。
	* cv
	* 三个重要的Principle。Occam's Razor， Sample Bias， Data Snooping.
	
## 我对这门课的收获

	* 霍夫曼定理
	* VC维的理解，线性相关
	* 略微有点啰嗦，为了避免数学，导致描述的复杂度上升好几个级别

总体来讲，对工程人员帮助不是特别大，但有利于加深概念的理解。

<To be continue>
