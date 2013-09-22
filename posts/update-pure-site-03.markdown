<!-- 
.. tags: 翻译, css
.. link: 
.. slug: update-pure-site-03
.. title: pure 0.3.0发布了，pure中文网站也更新了
.. description: pure, css框架, pure翻译, pure中文版
.. comment: True
.. date: 2013/09/21 11:12:14
-->




Pure发布新版本0.3.0也有一小段时间了，但最近一直在忙，没时间更新。现在趁着中秋放假，对中文网站做了一次同步更新，^_^!

先来看下pure0.3.0有哪些改变吧！

最大的改变，莫过于网格（grids）模块了！修复了上个版本两个比较严重的bug。

* 旧的IE浏览器上，网格单元格有时会被挤到新一行，具体可看<a href="https://github.com/yui/pure/issues/154/" target="_blank">issues154</a>

* 如果开发者使用了定制的字体，网格单元格也会被挤到新一行，具体可看<a href="https://github.com/yui/pure/issues/41/" target="_blank">issues14</a>

网格现在使用了CSS3的Flexbox，避免了对letter-spacing设置负数后可能产生的副作用。同时网格也使用了特定的字体栈去保证不同浏览器之间的兼容性。默认的网格都设置了	font-family:sans-serif;

如果你需要使用非默认的字体，pure推荐你这样子用

	body,
	.pure-g [class *= "pure-u"],
	.pure-g-r [class *= "pure-u"] {
	    /* Set you're content font stack here, for example: */
	    font-family: Georgia, Times, "Times New Roman", serif;
	}

其他改变：
* 上个版本使用pure时，是需要自己先手工引入Normalize.css；而当前版本已经把Normalize.css打包到基本模块里面，不需要再手工引入。

* 删除forms-core.css模块，因为这个模块是Normalize.css的一些form样式的复制，而当前版本已经包含了Normalize.css，所以不需要了。

* 给input添加readonly属性，设置了readonly属性的input是可以被focus的

* 给.pure-g-r下的图片添加height:auto规则，使得当调整页面大小的时候，它们的长宽比例能保持。

好了，点击 [http://pure-site.ap01.aws.af.cm/](http://pure-site.ap01.aws.af.cm/) 访问吧
