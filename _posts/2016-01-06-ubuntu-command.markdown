---
layout: post
title:  Ubunt常用命令总结
date:   2016-01-06 14:35:25
summary: 读书笔记
categories: tools
tags: ubuntu
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













