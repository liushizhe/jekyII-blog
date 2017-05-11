---
layout: post
title: jekyll baseurl url 详解
date: 2017-05-12 00:22:35
category: "jekyll"
---

jekyll本地调试时没问题，但是上传到github后图片无法显示，查看jekyll官方网站也没对 baseurl url 做详细描述，自己调试了下，写下一些见解，欢迎拍砖。（博客还不能评论，尴尬~~）

博客地址：https://acount-name.github.io/project-name/index.html  
_config.yml 文件设置 url、baseurl地址分别为  
url：https://acount-name.github.io/project-name  
baseurl：/project-name   #或为空，效果等效  

实际上，url就是博客绝对地址，baseurl为博客相对地址，若有需要引用其它链接文件，比如图片，则需加 url（绝对地址）或 baseurl(相对地址)，否则引用文件链接有误。
比如，  
{% highlight html %}
site.url/site.author.imageLink
或者
site.baseurl/site.author.imageLink
或者
site.author.imageLink | relative_url
{% endhighlight %}
site.author.imageLink：图片目录  
注意， site.author.imageLink | absolute_url 有问题，生成链接类似于  
site.url/site.baseurl/site.author.imageLink

测试jekyll版本 3.3.1，以上就是对于url baseurl理解。
