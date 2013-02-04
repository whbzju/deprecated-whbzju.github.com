---
layout: post
title: "Ubuntu 12.04配置Octopress在Github上搭建blog"
date: 2013-02-02 11:04
comments: true
categories: octopress 
---
  至从看到同学在Github上搭建的blog，深以为这才是我想要的blog，简洁漂亮，静态页面，离线编辑，markdown文档写作格式，git版本控制。所以当论文的事情告一段落，终于有时间来折腾它。起初，我在window平台上搭建，参考该[文献](http://shanewfx.github.com/blog/2012/02/16/bulid-blog-by-octopress/)配置，其中最大的问题是中文字符支持的问题。

##内容概述
* ubuntu 12.04配置octopress和github Page 
* git 配置问题，ssh key管理。
* 维护已经存在的github blog
* vim的markdown语法高亮插件设置和所见即所得设置

##ubuntu 12.04配置octopress和github Page 
  首先，我们来了解下概念问题，关于octopress，可以见下文：
> Octopress is a blogging framework which generates your enire blog in static files. Octopress has integrated Twitter, Google Analytics, Google Plus, Facebook and some other webservices. There are also good plugins for adding images, code, videos and other content into your blog posts. The framework is made for hackers and people who know something about Linux and shell.There are three official ways to deploy Octopress
Github Pages
Heroku
Rsync

大致的意思是说octopress是一个静态页面生成框架，具有一些列集成的功能。有三种发布方式：Github Pages，Heroku和Rsync，本文采用Github Pages。由于Octopress是基于Ruby实现，我对ruby没有接触，从别人blog中了解到一下关于ruby的几个重要概念
> Gem ruby的easy install，用来安装各种库，是用ruby写的，全称叫rubygems。
Bundler 基于gem的更高级管理工具，bundler相对于gem就好比apt-get相对于aptitude。不过他不是单纯的下载安装，他会根据本目录的Gemfile文件，把你缺少的包给装上。
Rvm Ruby Version Manager，用来安装各种版本的ruby，问题是ubuntu有apt-get，这个不大派上用场。
Rbenv Simple Ruby Version Management，也是用来安装各种版本的ruby。
Rake Ruby Make，顾名思义就是ruby写的make，他对应的Makefile是Rakefile
<!--more-->
###配置安装环境
ubuntu在默认环境下是没有octorpess的依赖环境，同时也缺少git工具。所以首先：
	sudo apt-get install bash curl git-core -y
	sudo apt-get install build-essential bison openssl libreadline6 libreadline6-dev zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-0 libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake -y
接着安装rvm，我们采用rvm来安装ruby和octopress的依赖环境。
`bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)`
同时我们还需要配置bash的环境，并重启bash
	echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # Load RVM function' >> ~/.bashrc
	source .bashrc
然后，配置octopress的环境：
	rvm pkg install openssl
	rvm pkg install iconv
	rvm install 1.9.2 -C --with-openssl-dir=$HOME/.rvm/usr,--with-iconv-dir=$HOME/.rvm/usr
	rvm use 1.9.2 --default
	rvm reload
	rvm rubygems latest
	gem install bundler
注意ubuntu 12.04默认的ruby版本是1.8.7，所以运行完上述命令务必要确认`ruby --version 来确认ruby的版本是否依旧修改成功。

###clone octopress
	git clone git://github.com/imathis/octopress.git octopress

	cd octopress/

###安装octopress
安装octopress依赖环境
	gem install bundler
	bundle install
安装octopress默认主题
	rake install

###部署到Github
部署过程比较简单，每一步都有详细的提示。首先你得现在github上创建一个repository，且必须命名成username.github.com.git，其中username是你的github的用户名，参见[用Github Pages搭建blog设置](http://beiyuu.com/github-pages/)。要注意的是ssh的管理，直接在github账户上添加ssh密钥，该密钥是作为deploy密钥，因此不能用于其他项目，也就是说，如果此时你有其他项目需要ssh密钥，该密钥已经不能被使用，你必须通过ssh-keygen重新生成密钥，并且通过ssh-agent进行切换。

用octopress写blog主要的步骤如下：
	rake new_post["title"]
	rake generate
这样静态页面就生成好了，可以通过`rake preview`来预览，访问`localhost:4000`进行测试。

没有问题就可以通过`rake deploy`部署到github上。

同时，不要忘了将source分支也上传到github上，依照默认的octopress配置，它是将source和master分开，在source分支下完成文章编写，通过deploy到master中，默认情况下github上只有master分支。但是如果你有多台电脑，需要维护一个github blog，其他电脑必须checkout 该项目的source分支才能进行修改，否则会出错。因此，必须将source分支也加入到项目目录中
	git add .
	git commit -m "origin source branch"
	git push origin source

如果一切正常，过一会你就可以通过`username.github.com来访问你的blog。

###octopress主题安装
我个人觉得默认主题不错，各位也可以安装其他主题，google吧。

##维护一个已经存在的github blog
首先，在机子上配置ruby环境，rvm，git和octopress的环境。

接着从github上clone你的blog项目，比如：
	git clone git@github.com:username/username.github.com.git
checkout source分支，这个是必须的步骤
	cd username.github.com
	git checkout source

重新配置本地octopress和Github Page的关联
	rake setup_github_pages
按提示完成设置

接着便于初次配置octopress一样的方式进行blog编写修改等。


##markdown编辑器的配置：VIM
   在linux平台上，vim是我首选的编辑器，不想换其他专门针对markdown语法的编辑器。在写blog的时候，我希望编辑器能够有两个功能:

* 支持markdown语法高亮
* 支持所见即所得模式编辑

google了下发现，可以通过安装vim-octopress和vim-instant-markdown插件来实现。
###vim-octopress配置
   建议采用[Pathogen](https://github.com/tpope/vim-pathogen)来安装vim插件。

	cd ~/.vim/bundle
	git clone https://github.com/tangledhelix/vim-octopress.git octopress
  若是没有设置过vim，先新建`~/.vimrc `和 `~/.vim/` 。最后在`.vimrc` 中加入，指定markdown的配色为octopress

	autocmd BufNewFile,BufRead *.markdown setfiletype octopress

###vim-instant-markdown插件配置
  该插件的功能是让你在撰写markdown文档时能立即看到成文效果，在安装完毕后，使用vim时自动启动浏览器，实时的展现你撰写的内容。安装步骤见项目的github[主页](https://github.com/suan/vim-instant-markdown)。我在安装该插件的时候遇到一些问题，还在解决中，希望能尽快使用它。

##总结&展望
  在折腾这个blog，分别在windows上和linux上都安装成功过，window上的中文字符集解决方法比较麻烦。linux下只要将语言设置到.bashrc即可。整个过程遇到多个ssh key管理问题，最后在github的help上找到ssh agent切换管理解决方案。ruby version不对，原因是ubuntu默认ruby版本为1.8.7，需要设置。在修改octopress中的其他文件，在git push时，需要用git add/rm 来处理这些文件后push。Github在build pages失败时，会有邮件提示错误原因，需要仔细看。
  总之，遇到问题先思考，有了思路后再针对性的查阅资料，尝试解决方案。
###接下来要解决的：
* 评论机制
* 代码高亮
* 主题修改
* 配置修改，config.yml文件等

##参考

[Octopress installation in Ubuntu 12.04 with rsync - Lennu.net](http://www.lennu.net/2012/05/11/octopress-installation-in-ubuntu-12-dot-04-with-rsync/)

[为已存在的Octopress配置环境](http://xingfuqiu.com/blog/ubuntu-update-to-1204/)

http://fancyoung.com/blog/octopress-study/

http://netwjx.github.com/blog/2012/03/18/octopress-note/

http://BeiYuu.com

[配置 Git 和 SSH 密钥连接 Github - CSSer](http://www.csser.com/board/4f53875c55bdcb545c000d05)

[解决cygwin下的“Could not open a connection to your authentication agent.”](http://www.cnblogs.com/cheche/archive/2011/01/07/1918825.html)

