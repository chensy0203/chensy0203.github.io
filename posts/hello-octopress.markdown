<!--
.. title: 用octopress写博客
.. slug: hello-octopress
.. date: 2013/02/24 15:45:00
.. tags: octopress, hello
.. comment: True
.. link:
.. description: octopress, windows7, GitHub Pages, 静态博客 
-->

## 写在前面

一直想用blog记录点什么东西，但用网上的blog服务，感觉不是很灵活。自己也用过wordpress，但它是基于数据库的，总感觉有所欠缺。直到遇见了cotopress，才发现这应该是我想要的了。它是用文件存储数据，以静态方式发布，可以直接挂在github等地方，而且是免费的。

这是第一篇博客，结合网上的教程与自己的搭建过程，做了下记录。以后如果遇到新的问题，也会不断在这里更新的。 ^_^

<!-- TEASER_END -->

-------------------


## 1.安装 Octopress
我的环境是：windows7

### 1.1安装 Git
	
去下载[Git for Windows](http://code.google.com/p/msysgit/downloads/list)

如何安装，可以参考下这里[在windows安装配置Git开发环境](http://www.starming.com/index.php?action=plugin&v=wave&tpl=union&ac=viewgrouppost&gid=32767&tid=1000002148)

安装完后，用Git clone 一份Octopress到本地，以下是命令

		git clone git://github.com/imathis/octopress.git octopress

### 1.2安装Ruby以及DevKit

去下载并安装[Ruby for windows](http://rubyinstaller.org/downloads/)

同时还要下载安装[DEVELOPMENT KIT](http://rubyinstaller.org/downloads/)。安装完后，还需要配置，可以参考[wiki](https://github.com/oneclick/rubyinstaller/wiki/Development-Kit)

### 1.3安装octopress相关的依赖

	gem install bundler
	bundle install	#要成功安装了DevKit后才能顺利安装。且注意是：bundle，而不是bundler
	rake install	#安装默认的Octopress主题。新建了三个文件夹：public sass source
	

rake install之后发生的事情
	## Copying classic theme into ./source and ./sass
	mkdir -p source
	cp -r .themes/classic/source/. source
	mkdir -p sass
	cp -r .themes/classic/sass/. sass
	mkdir -p source/_posts
	mkdir -p public

------------------


## 2.部署到Github Pages

### 2.1注册[github](https://github.com/)

Github Pages 可以提供一个域名为`http://username.github.com`的博客。

所以用户名就取好点了 ^_^

### 2.2配置github

首先在github新建一个名为`username.github.com`的[Repository](https://github.com/new)，然后就可以把Octopress博客部署到该域名上了。(该username必须与你在github上的用户名一致)

例如我的 GitHub 帐号是 chensy0203 就将 Repository 命名为 `chensy0203.github.com`， 完成后会得到一组 GitHub Pages URL `http://chensy0203.github.com`

### 2.3设定 GitHub Pages
	
	rake setup_github_pages

 以上执行后会要求输入repository的URL：`git@github.com:username/yourname.github.com.git`
	
发布blog
	rake generate		#生成静态页面
	rake deploy			#部署到github
	
等待几分钟后，你就可以到 `http://username.github.com` 上看到你的博客了.

###2.4日后维护

####2.4.1保存源码（将source加入git）

因为发布的只是生成的静态页面，需要在项目里建立source分支用于保存整个项目源代码（配置、markdown文件等）。
	
建立source分支
	git add .
	git commit -m 'initial source commit'
	git push origin source

####2.4.2更新 Octopress

日后有 Octopress 新版本发布，使用以下指令升级。

	git remote add octopress git://github.com/imathis/octopress.git
	git pull octopress master     # Get the latest Octopress
	bundle install                # Keep gems updated
	rake update_source            # update the template's source
	rake update_style             # update the template's style

------------------

## 3.开始写第一篇博文

	rake new_post["title"]		#生成一个markdown文件，名字类似为：2013-02-24-title.mrakdown

运行后会通过title生成相关文件。然后就可以用markdown去写博文了。

PS:windows下建议安装[MarkdownPad](http://markdownpad.com/)软件去写博文，挺不错的

写完后，就可以用以下的命令，在本地预览，没问题了就可以发布并部署到github上了

	rake preview			#启动服务器并监控变动，通过http://localhost:4000预览
	rake generate			#发布文件到public目录
	rake deploy				#部署到github

这时，我们会发现，在首页上，整篇博文都显示出来了。但如果只想要显示摘要怎么办？

1.在文中加入`<!--more-->`来控制摘要截取位置

2.修改`_config.yml`里的`excerpt_link`的值，可以设置链接的显示文字（默认是Read on）

---------------


## 4.个性化配置

###4.1修改Header, Navigation和Footer
可以直接编辑html文件，分别对应以下三个文件

`source\_includes\custom\header.html`

`source\_includes\custom\navigation.html`

`source\_includes\custom\footer.html`

###4.2添加新页面
	
	rake new_page[about]		#在source下新建about目录，并在里面添加index.markdown文件

然后编辑导航条`source/_includes/custom/navigation.html`

<ul class="main-navigation">
  <li><a href="{{ root_url }}/">首页</a></li>
  <li><a href="{{ root_url }}/blog/archives">存档</a></li>
  <li><a href="{{ root_url }}/about">关于</a></li>
</ul>


###4.3修改侧边栏

####以添加新浪新浪微博秀为例

在`_config.yml`文件中将`default_asides`处代码改为如下，即去除不要的侧边栏，并加入微博侧边栏。
	default_asides: [asides/recent_posts.html, asides/github.html, custom/asides/weibo.html]

到[http://app.weibo.com/tool/weiboshow](http://app.weibo.com/tool/weiboshow)获取自己微博秀的代码。
在`source/_includes/custom/asides`文件夹下新建`weibo.html`文件并编辑如下,将获取的微博秀代码插入到相应位置： 
	<section>
	    <h1>新浪微博</h1>
	    <ul id="weibo">
	    <li>

	    <!-- 在此插入获得的微博秀代码 -->

	      </li>
	    </ul>
	</section>

###4.4 分享到和评论功能

编辑`/source/_includes/post/sharing.html`,增加如下代码：

	//下面的大括号与百分号间无空格，如果复制，请去掉 
	{ % include custom/sharing.html % }

在`source/_includes/custom`下新建`sharing.html`文件

到[http://www.jiathis.com/](http://www.jiathis.com/)获取喜欢按钮样式的分享代码，加入到sharing.html中，即可增加分享到按钮功能。

到[http://www.uyan.cc/](http://www.uyan.cc/)获取评论功能的代码，加入到sharing.html中，即可增加评论功能。

###4.5 Octopress代码高亮
最简单的方式
	//下面的大括号与百分号间无空格，如果复制，请去掉  
	{ % codeblock  % }
	//你的代码
	{ % endcodeblock  % }

对于前端的源码还可以使用 jsFiddle插件，这个迟点有需求再研究

-------------

##5.问题收集（不断更新中）
###5.1执行git命令时，操作失败：Permission denied(publickey)
	原因：没有设置好ssh keys
	解决：参考[官方教程](https://help.github.com/articles/generating-ssh-keys)

###5.2**不能进行deploy(Github)**
	问题：有次发现rake deploy不能发布，但是预览正常。检查github上source分支代码已更新，但master仍为老代码。
	原因：发现是因为代码是新从github下clone下来的，未进行初始化deploy。
	解决：需要执行 rake setup_github_pages进行初始化。
	注意：rake操作应该在source分支下进行，若是刚从github里clone下来的，请先执行$ git checkout source。

问题实录：当我在本地对octopress进行了一系统的配置并且基本写完了第一篇博客后，想更新到github，突然发现`rake deploy`失败。
	## Deploying website via Rsync
	FAILED
之后试过重新`rake setup_github_pages`，但一样失败
	Enter the read/write url for your repository
	(For example, 'git@github.com:your_username/your_username.github.com)
	Repository url: git@github.com:chensy0203/chensy0203.github.com
	rake aborted!
	No such file or directory - git remote -v

	Tasks: TOP => setup_github_pages
	(See full trace by running task with --trace)
之后，在网上找了很多资料，进行过各种尝试，但依然不行。
最后学习了下git，用最原始的方法解决了。首先要明白，用rake deploy命令，只是把public文件夹下的静态文件更新到github上面。所以明白了这一点就好做了。
我在github上建立了一个空的仓库chensy0203.github.com，然后clone下来，之后，把clone的文件夹下的.git文件夹剪切到了public文件夹下
git clone git@github.com:chensy0203/chensy0203.github.com.git
git add .
git commit -m 'first commit'
git push origin master

###5.3**修改的样式preview时不生效**
	问题：预览时发现之前设置成功的自定义样式不生效，变回默认样式。
	解决：$ rake generate即可，会重新生成css。

###5.4**无法update octopress**
	每过一段时间，可能需要更新一下Octopress版本，
	问题：执行$ git pull octopress master时报错：fatal: 'octopress' does not appear to be a git repository
	原因：没有相应远程分支（第一次生成博客的项目中才有？）。打开.git/config查看，应该不包含[remote "octopress"]块。
	解决：手动添加，或者在命令行执行：$ git remote add octopress https://github.com/imathis/octopress.git

###5.5rake preview失败
	Starting to watch source with Jekyll and Compass. Starting Rack on port 4000
	E:/Ruby193/lib/ruby/gems/1.9.1/gems/bundler-1.3.2/lib/bundler/runtime.rb:33:in `block in setup': You have alrea
	dy activated rack 1.4.5, but your Gemfile requires rack 1.4.1. Using bundle exec may solve this. (Gem::LoadErro
	r)
执行下
解决方法：执行`bundle update; rake install`
----------------
###6.参考

* [Octopress官方文档](http://octopress.org/docs/)
* [技术博客利器——Octopress](http://fancyoung.com/blog/octopress-study/ "技术博客利器——Octopress")
* [用octopress来写博客](http://caok1231.com/blog/2012/06/24/install-octopress-to-write-blog/)
* [为Octopress修改主题和自定义样式](http://yanping.me/cn/blog/2012/01/07/theming-and-customization/)
* [为Octopress追加[分享到微博]按钮](http://programus.github.com/blog/2012/03/04/share-weibo-button/)
* [标签云](https://github.com/tokkonopapa/octopress-tagcloud)
* [使用scss來加速寫css吧](http://blog.visioncan.com/2011/sass-scss-your-css/)
* [利用Octopress在github Pages上搭建个人博客](http://easypi.github.com/blog/2013/01/05/using-octopress-to-setup-blog-on-github/)
