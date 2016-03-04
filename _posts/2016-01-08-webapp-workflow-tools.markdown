---
layout: post
title: 前端开发过程中的一般流程及常用工具
date: 2016-01-08 15:22:29
summary:
categories: js
tags: 流程 工具
---

学习MEAN开发过程中，一般的流程总结如下（从零到一）：

---
### 流程篇

1. 初始化一个项目
{% highlight sh %}
npm init
{% endhighlight %}

2. 安装依赖包，根据具体项目而定
{% highlight sh %}
# 常用的安装包有express,morgan,mongoose,PassportJS,body-parser
npm install express --save # 保存到package.json中
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

4. 前端模块化---通过AngularJS来构建
{% highlight javascript %}
module and route
ng-app
ng-view
ng-controller
ngRoute
{% endhighlight %}

---
### 工具篇

__Bower__(前端包管理软件)

[Bower](http://bower.io):前端包管理软件,可以用来下载CSS/JS库文件，例如常用的Bootstrap，Angular,jQuery,Animate.css等。工作机理和使用方法与与npm类似，npm使用package.json来配置，bower则用bower.json来配置。

使用方法：

{% highlight sh %}
# 安装bower
npm install -g bower

# 初始化
bower init

# 安装一个包，默认情况下会在当前目录生成一个bower_components的文件夹
bower install <package_name> --save

bower install bootstrap

# 通过github URL也可以安装
bower intall <github-url>

# 搜索一个包
bower search <package_name>

# 指定特定的目录放置包文件，而非默认的bower_components，编辑.bowerrc文件
{
    'directory':'public/assets/libs' # 这样子会报错，为什么？因为你TMD的用了单引号！！怪我喽

    "directory":"public/components/libs" # 运行成功
}

# 自定义
{
  "directory": "app/components/",
  "analytics": false,
  "timeout": 120000,
  "registry": {
    "search": [
      "http://localhost:8000",
      "https://bower.herokuapp.com"
    ]
  }
}

# 使用一个包
正常使用即可
{% endhighlight %}

__Gulp__(自动化工具)

[Gulp](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md):前端自动化工具，类似的还有Grunt，可以实现一些自动化工作，比如处理LESS或者SCSS文件，合并文件，minify文件等。

使用方法：

{% highlight javascript %}
//使用流程：下载包-->加载包--->配置使用
// 语法：gulp.task,gulp.src,gulp.dest,gulp.watch

// 安装
npm install -g gulp

// 指定在开发环中使用
// devDependencies 是开发时依赖，比如你模块用了mocha测试框架，那么你的模块的开发就依赖 mocha，如果别人想为你的模块贡献代码，他就需要安装mocha。但是只是使用你的模块的人，就不需要mocha。
// peerDependencies是为插件准备的。比如grunt的插件，里面没有“require("grunt")”，所以用dependencies会有问题。所以需要单独列出。

npm install --save-dev gulp
npm install gulp-less --save-dev
npm install gulp-minify-css gulp-rename --save-dev
npm install gulp-jshint --save-dev
npm install gulp-uglify gulp-concat --save-dev
npm install gulp-nodemon --save-dev
npm install gulp-git --save-dev // 关于git操作

// 类似的还有imagemin和clean


// 在项目根目录创建gulpfile.js

var gulp = require('gulp');
var less = require('gulp-less');
var minifyCSS = require('gulp-minify-css');
var rename = require('gulp-rename');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var nodemon = require('gulp-nodemon');

gulp.task('css',function(){
    return gulp.src('pulic/assets/css/style.less')
        .pipe(less())
        .pipe(minifyCSS())
        .pipe(rename({suffix:'.min'}))
        .pipe(gulp.dest('public/assets/css'));
});

gulp.task('js',function(){
    // 使用一个数组以及通配符传递多个文件
    return gulp.src(['server.js','public/app/*.js','public/app/**/*.js'])
            .pipe(jshint())
            .pipe(jshint.reporter('default'));
});

gulp.tast('scripts',function() {
    return gulp.src(['public/app/*.js','public/app/**/*.js'])
    .pipe(jshint())
    .pipe(jshint.reporter('default'))
    .pipe(concat('all.js'))
    .pipe(uglify())
    .pipe(gulp.dest('public/dist'));
});

gulp.task('watch',function(){
    gulp.watch('public/assets/css/style.less',['css']);

    gulp.watch(['server.js','public/app/*.js','public/app/**/*.js'],['js','scripts']);
});

gulp.task('nodemon',function(){
    nodemon({
    script:'server.js',
    ext:'js html less'
    })
    .on('start',['watch'])
    .on('change',['watch'])
    .on('restart',function(){
    console.log('Restarted!');
    })
});

gulp.tast('default',['nodemon']);


// 运行，默认default
gulp <task-name>





{% endhighlight %}


