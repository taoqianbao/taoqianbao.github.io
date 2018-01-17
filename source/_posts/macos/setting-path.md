---
title: MAC OSX 配置PATH变量
p: macos/setting-path
date: 2018-01-10 23:13:28
tags: [macos,path]
categories: [macos]
---

## 背景

本文主要阐述macos设置环境变量的问题, 
mac 一般使用bash作为默认shell

Mac系统的环境变量，加载顺序为：
    /etc/profile 
    /etc/paths 
    ~/.bash_profile 
    ~/.bash_login 
    ~/.profile 
    ~/.bashrc

当然/etc/profile和/etc/paths是系统级别的，系统启动就会加载，后面几个是当前用户级的环境变量。后面3个按照从前往后的顺序读取，如果~/.bash_profile文件存在，则后面的几个文件就会被忽略不读了，如果~/.bash_profile文件不存在，才会以此类推读取后面的文件。~/.bashrc没有上述规则，它是bash shell打开的时候载入的。

<!--more-->

## 详细介绍

以下在MAC OSX Yosemite 10.10上测试可用
在terminal中查看PATH变量的值

    echo $PATH

返回结果长成这个样子:(每个路径被冒号分割)

    /Applications/XAMPP/xamppfiles/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin

临时会话中修改PATH变量

如果只是想在当前terminal的会话中临时修改PATH变量则可以

    PATH=/Applications/XAMPP/xamppfiles/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/Users/Alex/.composer/vendor/bin

持久修改PATH变量

转到home目录 Home目录在哪里？ 在home目录中创建一个文件 .bash_profile

    nano .bash_profile

随后在其中加入

    export PATH=/Users/Alex/.composer/vendor/bin:${PATH}

重启terminal窗口后，再看看PATH变量就应该变了

    echo $PATH

    /Applications/XAMPP/xamppfiles/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/Users/Alex/.composer/vendor/bin



## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
