<!--
author: 板栗
head: 
date: 2017-02-19
title: supervisord 进程管理工具配置详解
tags: linux系统管理
images: 
category: linux系统管理
status: publish
summary: 
-->


> supervisor是用python写的一个进程管理工具，用来启动，重启，关闭进程。

### 1. 安装

```
#pip install  supervisor

//或者  centos7下

yum install supervisor
```

生成配置文件：  
下面这条命可以在etc目录下生成配置文件supervisord.conf, 同时可以检测supervisord 是否安装成功。 

```
# echo_supervisord_conf>/etc/supervisord.conf   
```

### 2.配置

配置文件解释如下

```
[unix_http_server]
file=/tmp/supervisor.sock ; UNIX socket 文件，supervisorctl 会使用
;chmod=0700 ; socket 文件的 mode，默认是 0700
;chown=nobody:nogroup ; socket 文件的 owner，格式： uid:gid

;[inet_http_server] ; HTTP 服务器，提供 web 管理界面
;port=127.0.0.1:9001 ; Web 管理后台运行的 IP 和端口，如果开放到公网，需要注意安全性
;username=user ; 登录管理后台的用户名
;password=123 ; 登录管理后台的密码

[supervisord]
logfile=/tmp/supervisord.log ; 日志文件，默认是 $CWD/supervisord.log
logfile_maxbytes=50MB ; 日志文件大小，超出会 rotate，默认 50MB
logfile_backups=10 ; 日志文件保留备份数量默认 10
loglevel=info ; 日志级别，默认 info，其它: debug,warn,trace
pidfile=/tmp/supervisord.pid ; pid 文件
nodaemon=false ; 是否在前台启动，默认是 false，即以 daemon 的方式启动
minfds=1024 ; 可以打开的文件描述符的最小值，默认 1024
minprocs=200 ; 可以打开的进程数的最小值，默认 200

; the below section must remain in the config file for RPC
; (supervisorctl/web interface) to work, additional interfaces may be
; added by defining them in separate rpcinterface: sections
[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

[supervisorctl]
serverurl=unix:///tmp/supervisor.sock ; 通过 UNIX socket 连接 supervisord，路径与 unix_http_server 部分的 file 一致
;serverurl=http://127.0.0.1:9001 ; 通过 HTTP 的方式连接 supervisord

; 包含其他的配置文件
[include]
files = relative/directory/*.ini ; 可以是 *.conf 或 *.ini
```

同时也说明一下这个配置文件各个部分之间的关系及相应的配置。 

1. `[unix_http_server]`  和 `[inet_http_server]` 是supervisord服务端的两种表现形式，即命令行和web界面， 一般配置一种形式即可，我用web界面比较方便，所以配置`[inet_http_server]`。  

2. 如果要开放到公网则 port的地址要写成 0.0.0.0， 这样公网才能访问正常。

3. [supervisord] 是 supersor 服务端的一些常规配置。 

4. [supervisorctl] 是supervisor 命令行客户端的配置，与`[unix_http_server]`配置对应，如果使用不使用命令行控制，则这一部分可以注释忽略。 配置的时候， 其中的user 和password 要和`[unix_http_server]` 里的用户名密码一致才能对接正常。 

5. [include] 一般放被管理的进程配置文件， 一般一个进程一个配置文件， 放在相应的目录下即可。

### 3. 使用

1.先开启服务端：

```
supervisord -c /etc/supervisord.conf
```

2. 再进入客户端 （命令行）

```
supervisorctl -c /etc/supervisord.conf

//客户端中相应的命令

> status    # 查看程序状态
> stop usercenter   # 关闭 usercenter 程序
> start usercenter  # 启动 usercenter 程序
> restart usercenter    # 重启 usercenter 程序
> reread    ＃ 读取有更新（增加）的配置文件，不会启动新添加的程序
> update    ＃ 重启配置文件修改过的程序
```

> 如果使用的Web界面管理， 则配置好`[inet_http_server]` 部分， 在浏览器中打开 ip:9001 ，输入 `[inet_http_server]` 中配置的用户名密码，就可以进入进程管理了。 


参考：  

[使用 supervisor 管理进程](http://liyangliang.me/posts/2015/06/using-supervisor/)

[supervisor 官方文档](http://supervisord.org/index.html)




