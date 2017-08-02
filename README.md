title: ' 使用mavo制作可自定义编辑的导航页面'
author: wangzc
tags:
  - mavo
categories:
  - WEB前端
date: 2017-08-01 16:03:00
---
离职后，得闲整理前端这一年的收获。发现每个浏览器的书签都是满满的，这些都是平常关注前端动态留下的痕迹。意外发现一个好玩的东西——mavo,今天就用它来做一个前端导航页面。

![webNav](http://ofi3qxlvd.bkt.clouddn.com/150163313034620rgmif1.png?imageslim)

预览地址：

导航数据存放在github上：[www.wangzc.cc/nav/index.html](http://www.wangzc.cc/nav/index.html)

导航数据存在本地：[www.wangzc.cc/nav/local.html](http://www.wangzc.cc/nav/local.html)

源代码：[https://github.com/wangzc92/webNav](https://github.com/wangzc92/webNav)

## 特点
有人会问，这页面跟之前的导航页面有啥区别？区别就是这个页面可以可视化编辑，添加删除排序都是轻而易举完成的。如果数据是存放在云上，编辑保存之后实时更新。

拿第一个内容存放在github上为例，打开页面之后，顶部右侧有个登陆键，使用github账号登陆之后，会出现下列选项：

![paste image](http://ofi3qxlvd.bkt.clouddn.com/1501635907601kdx3cum9.png?imageslim)

点击`Edit`则进入编辑状态，页面中所有的文字，图片，链接都可以修改，而且可以添加，删除，排序单站点导航或者一大类。修改完之后点击保存就自动同步到github上。

*说明：此处例子存储在我的github上，登陆你们的github是没有权限合并到我这里，点击保存后会将代码克隆到你们的版本库，将数据存储地址改成你们的之后就可以作为自己的导航页面了。如果你们有更多更好的导航，欢迎提交合并请求。*

## 代码
打开页面源代码，发现代码非常简单，加上html标签仅有35行，除了引用mavo的js之外，自己未写一行js。

```html

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<link rel="stylesheet" href="https://get.mavo.io/mavo.css">
	<link rel="stylesheet" href="style.css">
	<script src="https://get.mavo.io/mavo.js"></script>
</head>
<body>
	<main mv-app="webNav" class="mv-autoedit clearfix" mv-storage="https://github.com/wangzc92/webNav">
		<aside>
			<h1 property="name">前端导航</h1>
			<ul class="nav">
				<li property="nav" mv-multiple mv-value="category">
					<a href="#[title]">[title]<span>[count(item)]</span></a>
				</li>
			</ul>
			<footer>&copy;<a href="http://wangzc.cc" target="_blank">竹风欣海笑</a></footer>
		</aside>
		<article>
			<section class="clearfix" id="[title]" property="category" mv-multiple>
				<h2 property="title">导航标题</h2>
				<a class="item" property="item" href="" mv-multiple target="_blank">
					<div class="clearfix">
						<img property="img" src="">
						<h3 property="site">站点名称</h3>
					</div>
					<p property="info">简介简介简介简介简介简介简介简介</p>
				</a>
			</section>
		</article>
	</main>
</body>
</html>
```

细看代码，能够发现几个未见过的属性：`mv-app`,`mv-storage`,`property`,`mv-multiple`。没错，这些都是mavo中定义的属性。下面就讲解一下具体实现。

## mavo简介
官网：[http://mavo.io/](http://mavo.io/)

根据官网介绍，Mavo是通过扩展HTML的语法来管理、存储和转换数据的Web应用程序。特点有：

* 它可以将数据存储在本地或云中；
* 在网站上编辑数据，有一个直观的，自动生成的，可定制的界面。
* 多媒体在你的页面上通过拖放，粘贴，或者浏览，没有一行代码就可以上传。
* 在HTML中执行计算，在需要时进行更新。不需要编写JavaScript！

## 使用
使用方法也特别简单，只需下载或引用mavo的css和js。
```html
<link rel="stylesheet" href="https://get.mavo.io/mavo.css"/>
<script src="https://get.mavo.io/mavo.js"></script>
```

## 语法
mavo扩展属性较多，下面介绍几个最重要的，更多详见官方文档。

### mv-app
是mavo的根节点，可以理解成一个app应用的名字或id。

```html
<div mv-app="mavoTest">
	My first Mavo app!
</div>
```

### mv-storage
告诉mavo你的数据存放在哪里。可以存放在本地或云端。

* 本地：设置`mv-storage="local"`将数据存储在浏览器的localStorage中，但这种方式无法将你的数据分享给其他人，而且本地存储也是有容量限制的。
* github: 你不需要了解它是如何存储在github并从github上获取数据的，只需设置好存储位置即可。
	```
    https://github.com/[username]
    //mavo自动创建mv-data版本库，数据文件为[appname].json
    
    https://github.com/[username]/[reponame]
    //mavo自动创建[appname].json在指定版本库中
    
    https://github.com/[username]/[reponame]/[filename]
    //默认分支为master
   https://github.com/[username]/[reponame]/blob/[branch]/[filename]
   //设置不同分支
   ```
* 其他云端

### property
简单理解property定义的是该字段名。如果该节点定义的有class或id，可以直接拿来作为字段名，不需重复定义。

```html
<div property class="name">Lea Verou</div>
<div property id="name">Lea Verou</div>
<div property="name">Lea Verou</div>
```
### mv-multiple
如果说一个property是一个个体或字段，那么mv-multiple可以说是一个组或类。上面导航中有两个`mv-multiple`：一是单个站点组，由站点图标，站点标题，站点描述和站点链接组成。二是导航组，由多个站点组组成的大类。

### 其他属性
mavo还提供了很多属性，相互结合可以做出很复杂的页面，而代码却十分简单。更多属性例子请自行去官网查阅。

## 总结
利用mavo，曾做过可视化编辑多语言系统，解决编辑不懂代码的难题。

有了这个导航页面，随时可以添加内容，归类整理。最后还是那句话，如果你有更好的前端网站链接，欢迎编辑好发送合并请求，让前端导航更全。

>竹风原创，转载请附上原文链接：[http://wangzc.cc/blog/web-nav-mavo/](http://wangzc.cc/blog/web-nav-mavo/)