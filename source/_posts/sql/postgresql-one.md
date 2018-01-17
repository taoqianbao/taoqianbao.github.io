---
title: postgresql安装环境
p: sql/postgresql-one
date: 2018-01-11 12:17:01
tags: [sql,postgresql,pg]
categories: SQL
---

## 背景




<!--more-->

## 正文

``` shell
PeterMacBook:bin peter$ psql help
psql: could not connect to server: No such file or directory
	Is the server running locally and accepting
	connections on Unix domain socket "/tmp/.s.PGSQL.5432"?


PeterMacBook:bin peter$ ps -ef | grep postmaster
  501 46892 46159   0 12:01下午 ttys001    0:00.00 grep postmaster


PeterMacBook:10.1 peter$ brew services start postgres
==> Tapping homebrew/services
Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-services'...
remote: Counting objects: 14, done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 14 (delta 0), reused 9 (delta 0), pack-reused 0
Unpacking objects: 100% (14/14), done.
Checking connectivity... done.
Tapped 0 formulae (42 files, 55.2KB)
==> Successfully started `postgresql` (label: homebrew.mxcl.postgresql)
PeterMacBook:10.1 peter$ 
  
PeterMacBook:10.1 peter$ ps -ef|grep psql
  501 47471 46159   0 12:19下午 ttys001    0:00.00 grep psql

```

## 解决方案
``` shell
brew services start postgresql
brew services stop postgresql
```

## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
