<!--
author: 板栗
head: 
date: 2017-03-29
title: nginx禁止未绑定域名访问
tags: nginx
images: 
category: nginx
status: publish
summary: 
-->


> 除非注明，本站文章均为: nginx.cn原创，转载请注明本文地址: [http://www.nginx.cn/149.html](http://www.nginx.cn/149.html)

> 如果你想直接解决问题而不看其过程， 那就在你的nginx中加入下面的server配置。  

nginx 只允许某些域名访问 其他一律不能访问 ，是怎么写的？

对于这个问题可以参考官方文档

原文

In catch-all server examples the strange name “_” can be seen:

```
server {
    listen       80  default_server;
    server_name  _;
    return       444;
}
```

There is nothing special about this name, it is just one of a myriad of invalid domain names which never intersect with any real name. Other invalid names like “--” and “!@#” may equally be used.

 

解释：

在这个server段实例可以看到：奇怪的server_name名字“ _ “

```
server {
    listen       80  default_server;
    server_name  _;
    return       404;
}

```

其实这个名字没有什么特别的，它仅仅是一个许多无效的域名中的一个代表，与任何真实的名字永远不会相交。其它无效的名称，如“ - “ 和” ！@＃ “也可同样使用。
default_server：nginx的虚拟主机是通过HTTP请求中的Host值来找到对应的虚拟主机配置，如果找不到呢？那 nginx就会将请求送到指定了 `default_server` 的 节点来处理

对于未绑定的域名指向你的服务器时，匹配不到你配置的虚拟主机域名后，会默认使用这个虚拟主机，然后直接返回404。

把这个server段配置添加你的nginx.conf即可

打赏一下哈！

![](http://o8adahup8.bkt.clouddn.com/%E4%BA%8C%E7%BB%B4%E7%A0%81_meitu_1.jpg)


