---
layout: post
title: "机器学习技巧之feature_hashing"
date: 2015-02-10 15:58
comments: true
categories: 
---


## 问题


最近在玩kaggle上的ctr比赛，其训练数据含大量categorical，无法直接用LR模型。举个例子，某个categorical数据集含[苹果，西瓜，梨，桃子]四个类别，一般的处理方法是将这些类别映射成[0,1,2,3]，放入模型中训练。其实这是不合理的，在categorical中，桃子和西瓜并不存在等级差，而变成[1,3]后会存在3>1的问题。以Logistic Regression为代表的算法就无法对该特征学到合适的参数。因此，业界一般会对categorical数据集做onehotencoding，即向量化，还是以上面数据为例子，苹果对应的向量为[1,0,0,0]，桃子对应的为[0,0,0,1]。在sklearn中，可以通过OneHotEncoding或get_dummies实现。显而易见，数据会变得非常稀疏。同时，当categorical的类别变多，特征维度随之剧增，带来的内存存储问题。比如在这次的ctr中，如果采用OneHotEncoding，我60g内存的机器也会报Memory error。

再次，ctr领域或者说高维大数据领域，数据集或多或少的存在稀疏问题。主流ML库都会实现一套稀疏矩阵，应对该问题。feature hashing又称feature trick，类似于kernel trick，在ML领域得到广泛应用的技巧。
维基上的定义：

>In machine learning, feature hashing, also known as the hashing trick[1] (by analogy to the kernel trick), is a fast and space-efficient way of vectorizing features, i.e. turning arbitrary features into indices in a vector or matrix. It works by applying a hash function to the features and using their hash values as indices directly, rather than looking the indices up in an associative array


## 解决方案

维基上关于Feature Hash的描述非常清晰，各位自己去看，不再累赘，这里多说一点hash的方法。常见的有以下两种实现：

	function hashing_vectorizer(features : array of string, N : integer):
     x := new vector[N]
     for f in features:
         h := hash(f)
         x[h mod N] += 1
     return x
     
     
另外还有一种：

	function hashing_vectorizer(features : array of string, N : integer):
     x := new vector[N]
     for f in features:
         h := hash(f)
         idx := h mod N
         if ξ(f) == 1:
             x[idx] += 1
         else:
             x[idx] -= 1
     return x
     
     
可以理解，既然是hash，必然要付出collision时的代价。实现方案一并没有考虑处理冲突，N越长，冲突的概率越低，然后存储的要求会变大。实现二，通过有符号的hash来解决冲突问题，即有很大概率在出现冲突时，该hash值为0，即不起作用，更详细的描述参考文献2.
 
## sklearn FeatureHasher的实现


`class sklearn.feature_extraction.FeatureHasher(n_features=1048576, input_type='dict', dtype=<type 'numpy.float64'>, non_negative=False)`，该接口返回一个sparse类型的array。

> The hash function employed is the signed 32-bit version of Murmurhash3.

该接口需要注意的是数据入参，支持三种格式：pair、dict和string。可以参考官方的[featurehasher test](https://github.com/scikit-learn/scikit-learn/blob/master/sklearn/feature_extraction/tests/test_feature_hasher.py)。

stackoverflow上也有一个比较好的例子：

Q:

```
I am using FeatureHasher in scikit-learn.

Can anyone explain why I end up with 4 non zero data in the sparse matrix instead of 2 after the following:

>>> f = FeatureHasher(input_type='string')
>>> g = f.transform(('as','bs'))
<2x1048576 sparse matrix of type '<type 'numpy.float64'>'
with 4 stored elements in Compressed Sparse Row format>
>>> g=f.transform(('as','bs'))
>>> g.data
array([-1.,  1., -1., -1.])
>>> g.nonzero()
(array([0, 0, 1, 1], dtype=int32), array([341263, 354738,  98813, 341263], dtype=int32))

```

A:

```
It appears is expecting a sequence of sequences. The outer sequence being for the observations, and the inner being features. With your input, the inner sequence are the characters of the string.

Observation 0: 'a' -> 354738, 's' -> 341263

Observation 1: 'b' -> 98813, 's' -> 341263

Try this:

g = f.transform([['as'],['bs']])
For output:

>>> g.nonzero()
(array([0, 1], dtype=int32), array([494108, 335425], dtype=int32))
```


## 参考文献

[1] [Feature hashing From Wikipedia, the free encyclopedia](http://en.wikipedia.org/wiki/Feature_hashing)

[2] [Kilian Weinberger, Anirban Dasgupta, John Langford, Alex Smola and Josh Attenberg (2009). Feature hashing for large scale multitask learning. Proc. ICML.](http://alex.smola.org/papers/2009/Weinbergeretal09.pdf)

[3] [sklearn feature hashing](http://scikit-learn.org/stable/modules/feature_extraction.html)

