---
title: 全栈开发之NODEJS服务区搭建向导
tags: [全栈,nodejs]
p: /web/how-to-install-node-envs
date: 2018-02-12 12:12:16
categories: web
---

## 前言
现阶段要开发小程序，搭建服务器环境，所以本文主要用于安装服务器环境。

<!-- TOC -->

- [前言](#前言)
- [正文](#正文)
    - [环境要求](#环境要求)
    - [安装 Nginx](#安装-nginx)
    - [安装Node.js](#安装nodejs)
    - [开启 SFTP](#开启-sftp)
    - [配置 Nginx 和 HTTPS](#配置-nginx-和-https)
    - [部署代码](#部署代码)
- [小结](#小结)
- [关于作者](#关于作者)

<!-- /TOC -->


<!--more-->

## 正文

### 环境要求
服务器系统：CentOS 7.3 64位
数据库：MySQL 5.7

### 安装 Nginx
Node.js 是单进程的，我们可以通过多开 Node.js 并配合 Nginx 来实现多进程 Node.js 负载均衡，并且一些静态文件我们也可以直接通过 Nginx 代理，提高性能。其中第一步就是安装 Nginx。

通过 SSH 连接上云服务器，直接使用包管理工具 yum 安装 Nginx 即可：

``` JS
yum -y install nginx
```

安装完成之后会显示 Complete!，可以通过如下命令检查 Nginx 是否安装成功：
``` JS
nginx -v
```
这个命令会显示 Nginx 的版本号，如果显示如下信息，则安装成功：
``` JS
[root@VM_0_11_centos ~]# nginx -v
nginx version: nginx/1.12.2
[root@VM_0_11_centos ~]# 
```

### 安装Node.js
本文演示Demo 需要 7.6 以上版本的 Node.js 才能运行，目前最新版本为 8.x，yum 本身不提供 Node.js 的源，所以首先我们得切换源：
``` JS
curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -
```
接着就可以直接通过 yum 安装了：
``` JS
yum -y install nodejs
```
同理，我们可以通过如下命令验证 Node.js 是否安装成功：
``` js
node -v
```
该命令会返回当前 Node.js 的版本号，如果你看到了版本号大于 7.6，则 Node.js 安装成功：
``` js
[root@VM_0_11_centos ~]# node -v
v8.9.4
```

### 开启 SFTP

SFTP 是一种安全的文件传输协议，我们可以通过 SFTP 把本地的文件上传到服务器上，通过以下命令检查 sftp 状态：
``` js
service sshd status
```
看到输出的信息中有 active (running) 则表示 sshd 进程已经开启，可以通过 sftp 连接：

![service sshd status](/imgs/web/service-sshd-status.png)

接下来可以通过 FileZilla、Transmit 等 FTP 工具连接上服务器。

### 配置 Nginx 和 HTTPS
完成以上准备工作，就要开始配置 Nginx 和 HTTPS 了，首先需要申请一个 SSL 证书，可以到腾讯云申请免费的 [SSL 证书](https://console.cloud.tencent.com/ssl/apply)，申请成功之后下载证书，并把压缩包中 Nginx 目录下的证书文件通过 SFTP 上传到服务器的 /data/release/nginx 目录，如果没有这个目录则新建：

![sftp tools](/imgs/web/SFTP.png)

上传完证书以后，可以开始配置 Nginx，进入服务器的 /etc/nginx/conf.d 目录，新建一个 weapp.conf 文件，将文件拷贝到本地，打开编辑，写入如下配置（请将配置里 wx.ijason.cc 修改为你自己的域名，包括证书文件）：

``` JS
upstream app_weapp {
    server localhost:5757;
    keepalive 8;
}

server {
    listen      80;
    server_name wx.ijason.cc;

    rewrite ^(.*)$ https://$server_name$1 permanent;
}

server {
    listen      443;
    server_name wx.ijason.cc;

    ssl on;

    ssl_certificate           /data/release/nginx/1_wx.ijason.cc_bundle.crt;
    ssl_certificate_key       /data/release/nginx/2_wx.ijason.cc.key;
    ssl_session_timeout       5m;
    ssl_protocols             TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers               ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA;
    ssl_session_cache         shared:SSL:50m;
    ssl_prefer_server_ciphers on;

    location / {
        proxy_pass http://app_weapp;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
修改完将这个文件上传到服务器上，然后在 ssh 中输入：
``` JS
 nginx -t
```
如果显示如下信息，则配置成功：

![ngix test](/imgs/web/nginx-test.png)

配置成功之后，输入 nginx 回车，即可启动 Nginx。

此时通过配置的域名访问服务器，会显示 Nginx 详情页：

![nginx index page](/imgs/web/nginx-index.png)

如果访问 http://你的域名/weapp/a 会自动跳转到 HTTPS 上，并显示 502 Bad Gateway，则表示配置成功：

![nginx index page](/imgs/web/nginx-502.png)


### 部署代码
新建一个 [KOA](https://github.com/koajs/koa) 项目 
接着将 编译后的代码 目录下的所有文件都上传到 /data/release/weapp 目录下：
![sftp file list](/imgs/web/sftp-filelist.png)

使用 SSH 切换到代码目录：
``` JS
cd /data/releaes/weapp
```

输入以下命令切换 npm 源到腾讯云镜像，防止官方镜像下载失败：
``` JS
npm config set registry http://mirrors.tencentyun.com/npm/
```
接着安装全局依赖：
``` JS
npm install -g pm2
```
然后安装本地依赖：
``` JS
npm install
```
 
接着执行如下代码启动 Node.js
``` JS
node app.js
```
完成


## 小结

## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
