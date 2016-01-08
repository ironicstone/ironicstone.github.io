---
layout: post
title: 前端开发过程中的一般流程及常用工具
date: 2016-01-08 15:22:29
summary: 读书笔记
categories: js
tags: 流程 工具
---

学习MEAN开发过程中，一般的路程总结如下（从零到一）：

1. 初始化一个项目 
{% highlight bash %}
npm init
{% endhighlight %}

2. 安装依赖包，根据具体项目而定
{% highlight bash %}
// 常用的安装包有express,morgan,mongoose,PassportJS,body-parser
npm install express --save // 保存到package.json中
{% endhighlight %}

3. 开启服务
{% highlight javascript %}
var express = require('express');
var app = express();
var path = require('path');
var port = 8080; // 可以自定义

// 静态目录
app.use(express.static(_dirname+'/public'));

app.get('/',function(req,res) {
    res.sendFile(path.join(_dirname + 'path to your home page'));
});

app.listen(port);

console.log('打开localhost：'+port+'来访问');
{% endhighlight %}

4. 前端模块化---通过