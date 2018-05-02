<!--
author: 板栗
head: 
date: 2017-02-19
title: php7.0 编译过程问题详解
tags: linux系统管理
images: 
category: linux系统管理
status: publish
summary: 
-->



> 环境 ： CentOS Linux release 7.2.1511 (Core)


1. php 编译参数

```
# ./configure \
--prefix=/usr/local/php7 \
--exec-prefix=/usr/local/php7 \
--bindir=/usr/local/php7/bin \
--sbindir=/usr/local/php7/sbin \
--includedir=/usr/local/php7/include \
--libdir=/usr/local/php7/lib/php \
--mandir=/usr/local/php7/php/man \
--with-config-file-path=/usr/local/php7/etc \
--with-mysql-sock=/var/lib/mysql/mysql.sock \
--with-mcrypt=/usr/include \
--with-mhash \
--with-openssl \
--with-mysql=shared,mysqlnd \
--with-mysqli=shared,mysqlnd \
--with-pdo-mysql=shared,mysqlnd \
--with-gd \
--with-iconv \
--with-zlib \
--enable-zip \
--enable-inline-optimization \
--disable-debug \
--disable-rpath \
--enable-shared \
--enable-xml \
--enable-bcmath \
--enable-shmop \
--enable-sysvsem \
--enable-mbregex \
--enable-mbstring \
--enable-ftp \
--enable-gd-native-ttf \
--enable-pcntl \
--enable-sockets \
--with-xmlrpc \
--enable-soap \
--without-pear \
--with-gettext \
--enable-session \
--with-curl \
--with-jpeg-dir \
--with-freetype-dir \
--enable-opcache \
--enable-redis \
--enable-fpm \
--enable-fastcgi \
--with-fpm-user=www \
--with-fpm-group=www \
--without-gdbm \
--disable-fileinfo
```

> 编译参数详解参考 [CentOS 7.1编译安装PHP7](http://www.cnblogs.com/doseoer/p/5350944.html)


1. 问题

```
//  ./configure 操作执行完成后，执行make操作的时候出现错误：

collect2: error: ld returned 1 exit status
make: *** [sapi/cli/php] 错误 1

```
**原因： ** 
在安裝 PHP 到系统中时要是发生`「undefined reference to libiconv_open'」`之类的错误信息，那表示在「./configure 」沒抓好一些环境变数值。错误发生点在建立「-o sapi/cli/php」是出错，没給到要 link 的 iconv 函式库参数。

**解决方法：** 
make 操作之前，打开文件Makefile， 找到EXTRA_LIBS变量定义的位置，
`EXTRA_LIBS = ..... -lcrypt` 然后在行尾加上 -liconv，例如: EXTRA_LIBS = ..... -lcrypt -liconv 然后重新再次 make 即可。


参考：  

[安装PHP出现make: *** [sapi/cli/php] Error 1 解决办法](http://blog.csdn.net/sflsgfs/article/details/6318583)


