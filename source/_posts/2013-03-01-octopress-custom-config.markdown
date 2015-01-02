---
layout: post
title: "octopress custom-configuration 个性化配置"
date: 2013-03-01 09:25
comments: true
categories: octopress
---
#概述
我使用的octopress默认的主题，但是它的一些页面设置不能满足我的需求。比如：

* 导航栏
* 个人介绍页面
* 分类Categories
* sina微薄分享
* 评论

好在octopress的可定制性非常强，其核心配置文件叫 `_config.yml,  基本上的配置都需要用到这个文件。它的逻辑比较简单，blog所有的配置都在这个文件，它的基本格式如下：

```
# Disqus Comments
disqus_short_name: 
disqus_show_comment_count:
```

这是一个第三方的评论插件，通过简单的设置即可实现blog中加入评论。注意，该文件是基于yaml语法，：后面的空格不能省略。该文件还有许多其他的参数可以配置，其中本文主要关注的是：

```

# list each of the sidebar modules you want to include, in the order you want them to appear.
# To add custom asides, create files in /source/_includes/custom/asides/ and add them to the list like 'custom/asides/custom_aside_name.html'
default_asides: [asides/about.html, asides/weibo.html, asides/category_list.html, asides/recent_posts.html, asides/github.html, asides/twitter.html, asides/delicious.html, asides/pinboard.html, asides/googleplus.html]

```
如注释中提到，asides的设置，关联的目录在`/source/\_includes/custom/asides`。比如想要在右侧边栏中加入about me框，则需要在`/source/\_includes/custom/asides/`中新建about.html。建议借用该目录下默认的about.html。

```
<section>
  <h1>About Me</h1>
  <p>A little something about me.</p>
</section>

```
<!--more-->

#配置方法
博主的自定义主要参考网络上的几篇blog，在这里不再详述，实现的功能有:

* octopress navigation设置
* about页面，需要用rake new_page["about"]，先生成页面。
* octopress 侧边栏设置
* 个人介绍
* sina微博分享
* categories
* google analysis

# 建议
最好把里面的twitter相关的信息全部删掉，否则由于GFW的原因，将会造成页面load很慢。同理，修改定制文件/source/_includes/custom/head.html 把google的自定义字体去掉

#参考文献
[ctopress修改主题和自定义样式](http://yanping.me/cn/blog/2012/01/07/theming-and-customization/)

[octopress的个性化配置](http://linyi.herokuapp.com/blog/config-octopress.html)

[为octopress添加分类(category)列表](http://codemacro.com/2012/07/18/add-category-list-to-octopress/)

[为Octopress博客追加新浪微博侧栏](http://programus.github.com/blog/2012/03/03/add-weibo-sidebar-into-octopress/)

