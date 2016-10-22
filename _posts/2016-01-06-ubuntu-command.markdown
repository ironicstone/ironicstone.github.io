---
layout: post 
show:true
title:  Ubunt常用命令总结
date:   2016-01-15 16:21:27
summary:
categories: tools
tags: ubuntu
---

更新ftp部分内容

### ftp操作

__常用FTP工具__

- VSFTPD
- ProFTPD

__服务器端__

{% highlight sh %}

# 安装
sudo apt-get install vsftpd

# 配置
sudo vim /etc/vsftpd.conf
anonymous_enable=NO # 确保身份验证
local_enable=YES
write_enable=YES
chroot_local_user=YES # 确保只能访问指定目录

# 创建ftp目录
mkdir $HOME/files
chown root:root $HOME
# 在files目录进行相应操作

# 开启ftp服务
sudo service vsftpd restart
{% endhighlight %}

__客户端__

两种访问方式：

- 浏览器访问    ftp://example.com
- 命令行访问    ftp example.com

__ftp命令__

- put： 上传本地文件到服务器
- mput： 上传多个文件到服务器（可以上传前打包）
- get: 下载到本地
- mget: 同上
- ls
- cd
- help： 列出所有有用的指令
- pwd: print workspace directory of the remote
- delete: 服务器端删除一个文件
- mdelete
- eixt: 退出

__更加安全的文件传输方式__

sftp 使用方式与ftp类似,见参考链接

参考链接：

- [What is FTP and How Is It Used?](https://www.digitalocean.com/community/tutorials/what-is-ftp-and-how-is-it-used)
- [How To Set Up vsftpd on Ubuntu ](https://www.digitalocean.com/community/tutorials/how-to-set-up-vsftpd-on-ubuntu-12-04)
- [How To Configure vsftpd to Use SSL/TLS on an Ubuntu VPS](https://www.digitalocean.com/community/tutorials/how-to-configure-vsftpd-to-use-ssl-tls-on-an-ubuntu-vps)
- [How To Use SFTP to Securely Transfer Files with a Remote Server](https://www.digitalocean.com/community/tutorials/how-to-use-sftp-to-securely-transfer-files-with-a-remote-server)

---

### 课程介绍
应用场景：编辑器+控制台（Terminal）

现代开发者

### 你好，命令行

Root Directory --->根目录

绝对路径（/home/ironicstone）和相对路径(./ironicstone)

### 常用命令

- rm
- cp
- mv
- mkdir
- locate
- find
- grep

### 文件系统

__文件：__linux通过设置用户权限来隐藏文件，很多软件的配置文件因为平时用的很少因此通过在文件名前面加一个.记号，在使用ls命令不加参数就可以隐藏这样文件--->simplicity

__链接：__链接分为两种，硬链接和符号链接。硬链接实际上是为文件建一个别名，链接文件和源文件实际上是同一个文件；而符号链接建立的是一个指向，类似于快捷方式。硬链接只能用于文件，不能用于目录；符号链接可以为目录建链接。

__目录系统：__[详细解释](http://www.cnblogs.com/zf2011/archive/2012/05/17/2505847.html)

hd 硬盘/光驱等，sd USB盘，移动硬盘等。硬盘的第一个分区可能为/dev/hda1，USB盘可能是/dev/sda1，等等。一般来说，光驱是/dev/hdc。前面两个字母表示硬件中盘的类型，第三个字母表示盘的序号，例如a，b等。第四个数字指示传统意义上的分区。可以使用df命令查看硬盘使用情况，通过mount命令查看硬盘挂载情况

__用户权限：__用三个比特位来表示读（r），写（w），执行（x）的权限，那么我们可以用一个八进制数来表示。例如同时具有三种权限则为111，对应八进制数为7，可以通过chmod来更改。

### 常用命令

公共部分

公共参数：--help，--version。参数分为-开头和--开头，其中以-开头的是单字母参数。

Linux支持长文件名，所以在文件名有空格的时候你应该用引号包含整个路径，或者在空格前加上\符号。

目录控制

- cd
- mkdir [-p（多级嵌套，自动添加父级目录）]
- rmdir 只能删除空目录
- rm -r (可以删除目录)
- pwd 显示当前目录

文件管理

- ls
- dir 与ls功能完全相同，只是名字不同，兼容windows用户
- cat 输出一个文件，查看一个文件时可以使用，参数-n显示行号，-b显示非空行号，-E在每行结束时显示$字符   cat -n hello.c
- head/tail 依次显示文件的头十行，tail显示后十行。参数-c显示多少character，参数-n显示多少行。   head -n 3 hello.c 显示前三行
- mv [参数][源文件][目的位置+文件名（文件名可省略）] -f强行移动，-b如果目标文件存在就创造一个备份
- cp [参数][源文件]
- rm [参数][文件名/目录名] -i -r
- find
    - -name 查找文件名，含通配符×和？的文件名要用引号括起来
    - -perm 000 文件属性，用3位数字显示
    - -atime n n天之前访问过的文件
    - -mtime n n天之前修改过的文件
    - -ctime n n天前之间修改过
    - -newer filename 文件的最后修改日期较filename新则为真
    - -a and运算符
    - -o or运算符
    - ！ not运算符
    -exex command 执行command，command必须以“\;”记号结尾,{}表示前面处理过程中过滤出来的文件。 find . -name hello.c -exec cat {}\; 寻找当前目录及其子目录下是否存在hello.c，如果有的话，用cat输出

压缩解压

- gzip/gunzip [参数][文件名]
- bzip2/bunzip2
- tar [参数][文件名] 常用 tar -cjvf -xjvf -czvf -xzvf
    - -A 将文件增加到tar包里面
    - -c 新建tar包
    - -d 比较tar包和文件系统里面对应的文件
    - -delete 删除tar包中的内容
    - -t 列举tar包中的内容
    - -r 在包的末尾添加文件
    - -x 将文件从tar包中解压
    - -f 指定操作的tar文件名称
    - -h 不包含链接文件，而是加入他们指向的真实文件
    - -j 使用bzip2压缩之后再加入tar包
    - -z 使用zip/unzip处理tar文件
- zip/unzip
- rar/unrar
- 7z

文件比较

- cmp
- comm
- diff

管道符号|之后的常用命令

- command|more 页
- command|less
- command|grep

shell增强命令

- history 显示你输入的历史
- whereis 搜索系统命令
- which

程序运行控制

- &符号，表示这个程序在后台运行
- CTRL-C 终端当前程序，回到shell
- CTRL-Z 暂停当前程序，回到shell
- ps -eo pid,comm,%cpu,%mem
- kill pid
- nice 调整优先级（-20-20之间，越小优先级越高） nice -10 eva

### 管理命令

- sudo
- useradd
- userdel
- chmod
- mount
    - mount /dev/hdc /cdrom -o loop
    - mount /dev/hda3 /media/hda3
- unmount
- fsck 检查文件系统
- df diskfree 显示磁盘剩余空间 df -h -T
- cfdisk
- mkfs
- shutdown [参数][时间（now）]-r 重启 -h 关机 -n 快速关机

### Ubuntu包管理系统

apt：advanced package tools

apt-get [参数][包的名称]

- install
- remove
- autoremove 删除系统没有用到的软件包
- clean 删除缓存里的旧包
- update

aptitude

### 系统核心配置文件

源地址的配置 /etc/apt/source.list

[推荐源参考](http://wiki.ubuntu.org.cn/%E6%BA%90%E5%88%97%E8%A1%A8)

备注：注意不同版本之间关键词的替换

### GNU编译环境

g++

### 命令行下的Internet

- ping
- wget 下载
- w3m

---
### ubuntu系统安装相关

制作U盘启动盘

分区方案：

将home目录作为一个单独的挂载点，home就是默认的工作目录，也是用户所在的地方。重装系统或者升级系统，/home分区的数据都可以得到保留，最大程度避免了软件安装和重新配置的耗时工作。备份时只备份home分区，避免全盘备份。

所以一般来说一个 Ubuntu 的系统在安装之初应该有三个分区，他们分别是挂载于根目录： /, home目录： /home 的两个分区以及 swap 分区。swap分区是指虚拟内存的交换区，一般设置为实际内存容量的两倍大小即可。

假设你有一台500G硬盘，2G内存的PC，那么比较好的分区分配方案是：根目录所在分区分配50G左右， swap分区分配4G，剩余空间全部留给 /home 所在分区即可。

### 打造Ubuntu系统的基础设施

- 高速下载软件，选择合适的源。可以选择国内的官方源比如阿里云的（编辑/etc/source.list,或者选择最佳服务器），163的，教育网的用户务必选择高校提供的源，速度十分给力。下载特定软件时会用到第三方源，需要时再添加。Canonical合作伙伴的源也尽量选中，方便后面的安装。
- 安装显卡驱动[Nvidia](http://www.nvidia.com/object/unix.html)
- 同步系统的建立
    - 在云端保存基于单一文本的linux知识库，随时记录新的指示点，尽可能简洁，KISS原则，Keep it simple, stupid!
    - 在云端保存一个软件安装的脚本
    - 在云端备份home目录下重要的配置文件，例如.vim .vimrc 保存了gvim的配置信息，.bashrc保存了shell的配置信息
- 借助代理服务器，跨域高墙

{% highlight sh %}
# GUI程序可以直接配置，命令行程序按如下步骤配置

# 情况一：已有代理服务器的ip和端口号
sudo apt-get install proxychains -y
sudo vi /etc/proxychains.conf
注释掉文件中最后一行：socks4 127.0.0.1 9050
幷在文件最后追加一行：socks5 proxy_ip_address port

# 情况二 设置本地计算机为代理服务器，这种情况下你有一台装有ssh server的国外主机
ssh username@machine_d_ip_address -D 127.0.0.1:7070 # 建立 ssh channel，并且不要关闭这个终端或者退出 ssh

# 更改上述文件，ip和端口号为127.0.0.1,7070
{% endhighlight %}

### Ubuntu系统备份恢复升级策略

---

### 番外篇：有意思的命令行工具

- [来自知乎-如何善加利用Terminal](https://www.zhihu.com/question/29442452)
- [Github-Awesome CLI](https://github.com/Voyga/awesome-cli)

__[wego](https://github.com/schachmat/wego)运行在命令行中的天气app__

初步摸索了一下，大致步骤如下：

- 安装go环境，sudo apt-get install golang
- 配置GOPATH和GOROOT
{% highlight sh %}
# 打开 vim ～/.bashrc,加入下面配置，具体目录可以自己定义
export GOROOT=/usr/lib/go # go 的安装目录
export GOPATH=$HOME/go # go 项目的目录

# 添加完毕以后，保存并退出，运行go env查看go的环境配置，可以看到GOPATH和GOROOT配置成功。
{% endhighlight %}
- 以上完成以后，安装wego，运行 go get github.com/schachmat/wego
- GOPATH目录下就有源文件了，我的是$HOME/go/src/github.com/schachmat/wego,这里面有个we.go文件，运行一次，go run we.go,会报错同时在$HOME目录下生成.wegorc配置文件，并且提示你缺少天气网站的API key。
- 到[worldweatheronline](https://developer.worldweatheronline.com/auth/register)去申请你的key，可以链接github或者重新注册一个账户。登陆之后，可以获得API KEY
- 将你获得的key填入到.wegorc中，并设置默认的城市和语言。我的配置如下：
{% highlight sh %}
{
    "APIKey": "Your API key",
    "City": "Wuhan", # 填入默认城市
    "Numdays": 3, # 填入默认获得的天数
    "Imperial": false, # 华氏温度
    "Lang": "zh" # 中文显示
}
{% endhighlight %}
- 到此为止，再次运行 go run we.go即可成功显示天气，因为指令较长，使用alias简化命令。
{% highlight sh %}
# 函数可以接受参数，$@表示输入的参数
alias wego='_(){ go run $HOME/go/src/github.com/schachmat/wego/we.go "$@";}; _'
{% endhighlight %}
- 现在可以直接在命令行中敲wego啦，同时它可以接受两个参数，比如wego beijing 5,显示北京未来5天的天气。

上一张效果图：

![wego效果图]({{site.baseurl}}/image/test/wego.png)

---

### 常用软件命令技巧

1 sdcv（startdict的命令行版本）

{% highlight sh %}
#安装方法

#1
sudo apt-get install sdcv
#2安装字典
到http://abloz.com/huzheng/stardict-dic/zh_CN/ 下载需要的词库
然后sudo tar -xjvf stardict-kdic-computer-gb-2.4.2.tar.bz2
接着sudo mv stardict-kdic-computer-gb-2.4.2 /usr/share/stardict/dic/
#3放心食用
{% endhighlight %}

2 zsh+pure+code highlight

参考链接

- [oh my zsh](https://github.com/robbyrussell/oh-my-zsh)
- [pure](https://github.com/sindresorhus/pure)
- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

{% highlight sh %}
# zsh v4.3.9以上，zsh --version检查
sudo apt-get install zsh
# 设置zsh为默认shell
chsh -s $(which zsh)
# 注销后重新登陆，并且检查zsh是否为默认shell
echo $SHELL
# 安装oh my zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
# 安装主题pure
npm install --global pure-prompt
# 安装语法高亮插件
git clone git://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting

plugins=( [plugins...] zsh-syntax-highlighting)

source ~/.zshrc
{% endhighlight %}

3 主题,图标和docker栏

参考链接

- [Numix(推荐)](https://numixproject.org/)
- [Paper](http://samuelhewitt.com/paper/download)
- [简书推荐](http://www.jianshu.com/p/617e4388d814)
{% highlight sh %}
# 安装Unity Tweak Tool
sudo apt-get install unity-tweak-tool

#安装主题和图标
sudo add-apt-repository ppa:numix/ppa
sudo apt-get update && sudo apt-get install numix-gtk-theme numix-icon-theme-circle

# paper主题
sudo add-apt-repository ppa:snwh/pulp
sudo apt-get update && sudo apt-get install paper-icon-theme paper-gtk-theme

# Plank Docker
sudo add-apt-repository ppa:ricotz/docky
sudo apt-get update && sudo apt-get install plank
{% endhighlight %}

4 RSS阅读器

{% highlight sh %}
sudo add-apt-repository ppa:quiterss/quiterss
sudo apt-get update && sudo apt-get install quiterss
{% endhighlight %}

5 截图工具shutter

{% highlight sh %}
sudo add-apt-repository ppa:shutter/ppa
sudo apt-get update && sudo apt-get install shutter
{% endhighlight %}

6 安装文泉驿字体

{% highlight sh %}
sudo apt-get install -y ttf-wqy-zenhei
{% endhighlight %}

7 视频播放器和音频播放器

Ubuntu中的视频播放器选择VLC，音频播放器有两个选择，都是基于命令行的。

- [cmus](https://github.com/cmus/cmus)
- [musicbox](https://github.com/darknessomi/musicbox)

界面示例

![cmus]({{site.baseurl}}/image/201601/cmus.png)
![musicbox]({{site.baseurl}}/image/201601/musicbox.png)

Ubuntu中Chrome浏览器安装Flash Player

{% highlight sh %}
sudo apt install  adobe-flashplugin
{% endhighlight %}

8 配置ShadowSocks

{% highlight sh %}
# 安装shadowsocks客户端，没有pip就先安装pip
sudo apt-get update
sudo apt-get install python-pip
sudo apt-get install python-setuptools m2crypto

# 使用pip安装或者直接apt install
pip install shadowsocks
sudo apt install shadowsocks

# shadowsocks配置（json）格式
{
"server":"11.22.33.44",
"server_port":50003,
"local_port":1080,
"password":"123456",
"timeout":600,
"method":"aes-256-cfb"
}

# 添加到开机启动，添加到/etc/rc.local即可
sslocal -c ~/.config/shadowsocks/shadowsocks.js -d start

{% endhighlight %}





