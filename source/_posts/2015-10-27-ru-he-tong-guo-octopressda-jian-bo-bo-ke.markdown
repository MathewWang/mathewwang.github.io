---
layout: post
title: "如何通过octopress搭建博客"
date: 2015-10-27 23:38:23 +0800
comments: true
categories: summary
---
---
工作几年了，突然发现自己居然没有博客，这怎么行！！！前几天看到同事们的博客都搞的很像模像样的，自己也手痒，就花了两天时间用octopress＋github pages搭了一个，至于为什么用octopress，当然是因为唐巧大神也在用啊，从他那边看了好多很不错的文章，深感敬佩。既然是需要自己动手做，自然会遇到一些莫名奇妙的问题，下面就是介绍我自己是如何搭的：
##准备工作
Mac一台并且配好了github SSH访问权限，已注册好github，可以正常访问github的网络
##Github pages配置
关于Github pages的配置在[Github pages](https://pages.github.com)官网有很详细的介绍，按照上面的步骤一步一步来就好了。这里假设你的github pages的路径是“user_name.github.io”.
##Ruby环境安装
ruby的安装有多种方式，我这里用的是rbenv来安装的：
###rbenv安装
可以参考[rben安装](http://octopress.org/docs/setup/rbenv/)

	 cd
	 git clone git://github.com/sstephenson/rbenv.git .rbenv
     echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
     echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
     git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
     source ~/.bash_profile
###安装ruby
我这里使用ruby的2.2.3版本

	rbenv install 2.2.3
	rbenv local 2.2.3
	rbenv rehash  //这个命令很重要

##octopress安装和配置
重头戏来了，这里就是安装octopress,我在这里卡了好久，octopress是从github上直接拉去的，请参考下面的命令：

	git clone git://github.com/imathis/octopress.git octopress
	cd octopress
	gem install bundler
	rbenv rehash    # 这个一定执行
	bundle install
	rake install  #这个命令执行完之后会在octopress目录下面生成一个“source”文件夹

####这里有几个需要注意的地方：
1.在执行gem之前先执行下面的命令将gem的源换成伟大的淘宝源：
	
	gem sources -l #查看当前的gem源
	gem sources --remove https://rubygems.org/
	gem sources -a https://ruby.taobao.org/  #注意这里已经变成https了，现在网上还有很多教程用http是不对的
2.执行完上面的操作之后你会发现为什么我的gem命令还是去[https://rubygems.org/](),这是因为octopress项目里面的Gemfile里依然用的还是用之前的，并且项目的优先级是高于全局配置的，这里只要修改一下就可以了。

3.最后在执行“bundle install”的时候，我遇到了一个报错说“RedCloth －v ‘4.2.9’” 安装失败，不太清楚具体的原因。。。然后我就想了一个方法绕过去，先单独执行“gem install RedCloth －v ‘4.2.9’” 将它安装成功，然后将这个依赖从octopress的Gemfile中删掉就好了。
##关联github和本地的octopress

	rake setup_github_pages
执行这个命令时会需要输入github pages对应的那个repos路径，按照提示输入并确定即可。执行完之后会在octopress目录上生成一个“_deploy”的目录，之后生成的博客内容都是在这个文件夹上，另外这里还是将octopress目录指向你github pages repos对应的新source分支，并且将“_deploy”指向master分支。以后博客的内容(_deploy目录里的文件)会被push到master分支，而其它的文件会被放到source分支，当你在新的电脑上写博客的时候直接配置好本地的octopress环境，然后用github上的source和master分支覆盖本地的对应目录然后执行“rake setup_github_pages”就又可以愉快的写博客啦。
##简单blog发布
所有的配置工作准备好了之后就是写博客和发布博客啦，octopress使用了rake的下面几个task来完成博客的html文件生成和发布，具体可以参考[这里](http://octopress.org/docs/blogging/)：
	
	rake new_post["blog_title"]   #这个命令就是生存博客的框架，是一个markdown文件，写博客的时候直接修改这个文件就可以
	rake generate  #将上面的markdown文件生成对应的html文件
	rake preview   #博客预览，端口是4000，如：localhost:4000/blog/archives/index.html
	rake deploy    #将生产的博客发布到你前面设置的对应的github page repos
####这里有几个注意的地方
当需要deploy的时候最好是进到“_deploy”目录。如果push失败可以自己手动pull之后再push。当需要将博客正文以外的其它文件提交到source分支最好是进到“source”目录。因为octopress对应两个分支，在提交的时候应该要注意一些。

关于博客页面如何调整以及如何添加分享入口和评论，请参考下面唐巧的博客。
##引用
这里有一些可以参考的文章

[唐巧的技术博客](http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/)


