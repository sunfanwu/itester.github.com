---
layout: post
title: "Github博客指南"
description: "这篇是很久之前就说过要写的，可是一直懒得写，最近再来看，发现一切都变得简单了。所以就有了你看到的这篇Blog，我在这里讲述了怎么用最简单的办法建立和编辑github blog"
category: "指南"
tags: [Github, 指南, Jekyll]
published: true
---
###为什么写blog

####Blog的目的

我在本blog的[第一篇文](http://itester.me/hello-world/)里就有提到过。现在理由有多了一条：[Web 2.0时代我们需要什么样的阅读](http://www.williamlong.info/archives/3135.html)。微博，人人，每个人都在转发，但是在我看来，这些东西的信息量太小了，小的没有什么意义。只有经过加工的博文才是真正有营养的信息。

####看完本文你能得到什么

一个看起来还不错的Blog

###Github配置

####创建repo

你可以把一个repo理解为一个仓库，里面放置着你关于一个项目的代码。要建立一个机遇github的blog，你需要建立一个username.github.com的repo。这个形式的repo每个用户只能建立一个。

####添加SSH密匙

如果是以前，这一步要先安装git，再创建密匙，再上传。

现在，你只需要下载并安装[Github for Windows](http://github-windows.s3.amazonaws.com/GitHubSetup.exe)

登录后，他会帮你搞定其他的。

####初始化你的Blog

你的blog repo现在还是空的，我们要给他建立目录结构。当然不是让你手动去创建，因为我们有[Jekyll-Bootstrap](http://jekyllbootstrap.com/)。这个网站会帮你生成你的网站的代码。他甚至会生成含有你的用户名的，专为你定义的代码。所有你需要做的，就是把它们一行一行Copy到Shell里，回车。

####更改config

Jekyll-Bootstrap帮你建立的blog还有一些地方需要你自己来定义。

打开_config.yml,把`title`、`tagline`、`name`、`email`修改为你自己的信息。

注意`:`后面要留一个空格。级的push你的修改。

现在，你的blog已经建立起来了。快到username.github.com访问看看吧

###怎么写Blog

####The Easy Way

直接使用[Prose](http://prose.io)。这是一个在线服务，他可以接入你的github账户，修改文件。所以你需要做的就是进入`_post`文件夹，新建一个`.md`文件然后编辑他。发布的时候记得勾选published。

因为Prose使用了和Jekyll相同的解析格式，所以你在Prose所预览到的效果，就是你最终得到的效果。我的这篇Blog就是在Prose下完成的。

当然，如果你放弃了markdown，只是使用html tag来写blog，就像[Oilbeater](http://oilbeater.com)，Prose可能不能提供好的预览

####The Pro Way

虽然Prose很方便，但是如果你想更全面的掌控你的blog，包括自定义CSS等等玩意，你需要在本地安装Jekyll，来金星本地预览。

你可能会说，不需要，我只要做出修改，Push，在线查看效果就行了。相信我，安装Jekyll之后，开启服务器你就可以不断的刷新来即使查看效果，而上传，你需要三个步骤。一个字，麻烦。

要安装Jekyll，你需要

1. 安装Rails

	我试过不同的方法在windows下安装rails，比较后找到了其中最方便的。  
    下载[railsinstaller-2.0.1](http://kuai.xunlei.com/d/ADWIMODJRJBB)/[备用链接](http://115.com/file/dpca6xj8#railsinstaller-2.0.1.exe)
    
2. 安装jekyll
	
    在shell里输入`gem install jekyll`  如果你的网速有问题，多试几次。
    
3. 安装Ridiscount

	默认的markdown解析maruku对于中文的支持不是很好，我们把它替换成Ridiscount。
    在shell里输入`gem install ridiscount`
    
    在_config.yml里`pygments: true`，在下面添加一行`markdown: rdiscount`
    
4. 设置UTF-8

	在Windows下，jekyll会出现乱码或启动不成功的问题，是因为Windows对于UTF-8的支持不是很好。但是伟大的程序猿们已经找到了解决办法。
    
    打开`C:/RailsInstaller/Ruby1.9.2/lib/ruby/gems/1.9.1/gems/jekyll-0.11.2/lib/jekyll/convertible.rb`，把其中的`self.content = File.read(File.join(base, name))`替换为`self.content = File.read(File.join(base, name), :encoding => "utf-8")`
    
    在系统变量里添加两组变量：`LC_ALL=zh_CN.UTF-8`和`LANG=zh_CN.UTF-8`
    
现在你可以进入你的github blog目录，使用`jekyll --server`来开启本地jekyll服务器预览blog效果。

如果你没有成功，或遇到了其他问题，可以试着在[Jekyll 本地调试之若干问题](http://chxt6896.github.com/blog/2012/02/13/blog-jekyll-native.html)中看看有没有你的情况。
    
###Todo

- 绑定独立域名

- CSS以及主题

- 更多细节

###参考文章

如果你想了解更多Github以及Jekyll，可以看一下一下文章

- [用Jekyll构建静态网站](http://chen.yanping.me/cn/blog/2011/12/15/building-static-sites-with-jekyll/)官方文档的译文版

- [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)

- [使用Github Pages建独立博客](http://beiyuu.com/github-pages/)
	






