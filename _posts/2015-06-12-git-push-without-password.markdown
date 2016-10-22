---
layout: post 
title:  "Git push避免输入密码"
date:   2015-6-20 20:15:00
summary: 在使用git push往github上上传代码是避免输入账号和密码的方法
categories: tools
tags: github git
show: true
---

###1 创建文件存储GIT用户名和密码
在%HOME%目录中，一般为C:\users\Administrator，也可以是你自己创建的系统用户名目录，反正都在C:\users\中。文件名为.git-credentials,由于在Window中不允许直接创建以"."开头的文件，所以需要借助git bash进行，打开git bash客户端，进入%HOME%目录，然后用touch创建文件 .git-credentials, 用vim编辑此文件，输入内容格式：

{% highlight bash %}
touch .git-credentials
vim .git-credentials
https://{username}:{password}@github.com
//eg:https://a@b.com:12345@github.com
{% endhighlight %}

###2 添加Git Config内容
进入git bash终端，输入如下的命令：
{% highlight bash %}
git config --global credential.helper store
{% endhighlight %}

执行完后查看%HOME%目录下的.gitconfig文件，会多了一项：
{% highlight bash %}
[credential]
    helper = store
{% endhighlight %}
重新开启git bash会发现git push时不用再输入用户名和密码

使用credential.helper进行配置
{% highlight sh %}
$ git config --global credential.helper 'cache --timeout=3600'
# Set the cache to timeout after 1 hour (setting is in seconds)
{% endhighlight %}
