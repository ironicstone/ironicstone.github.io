---
layout: post
title:  "目录分析"
date:   2015-06-18
summary: Jekyll目录设置
categories: jekyll
tags: jekyll
---

####测试目录树的配置

{% highlight javascript %}

var a = 0, b = "China";
for(var i = 0 ; i < 10000; i++)
{
console.log("Here I am");
}

{% endhighlight %}

####jekyll中的常量
- 全局变量
	- site
	- page
	- content
	- paginator

- 站点变量
	- site.time
	- site.pages
	- site.posts
	- site.related_posts
	- site.categories.CATEGORY
	- site.tags.TAG

- 页面变量
	- page.content
	- page.title
	- page.excerpt
	- page.url
	- page.date
	- page.id
	- page.categories
	- page.tags
	- page.path
	- page.CUSTOM

- 分页
	- paginator.per_page
	- paginator.posts
	- paginator.total_posts
	- paginator.total_pages
	- paginator.page
	- paginator.previous_page
	- paginator.previous_page_path
	- paginator.next_page
	- paginator.next_page_path
- 测试
- 测试2
- 测试3

<input type="button" value="点我点我" onclick="pop()">

<script>
	function pop () {
		alert("测试成功");
	}
</script>

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="http://music.163.com/outchain/player?type=2&id=385781&auto=1&height=66"></iframe>




