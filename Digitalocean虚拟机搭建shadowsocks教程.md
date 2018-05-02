<!--
title: Digitalocean虚拟机搭建shadowsocks教程
author: 板栗
category: tech
tags: Digitalocean
-->

> 	相信很多朋友，特别是IT界的朋友，在查找各类国外文献的时候，就需要代理的帮助。在这里，我就和大家分享一下如何通过digitalocean云主机来搭建自己的shadowsocks代理,对IT有兴趣的朋友，也可以通过自己的云主机，折腾一些自己感兴趣的东西。。那我们就开始了

1. 首先，我们要先拥有一台自己的digitalocean的云主机。（本来有一台阿里云，但是比国外的贵多了，还不能搭代理，经朋友推荐选择了digitalocean，现在觉得用用还阔以，性价比也高。）

	- 登录digitalocean的主页注册一个自己的digitalocean账号。

	[https://www.digitalocean.com/?refcode=ffe6d8b9a0ee](www.baidu.com)
	
	![image](http://i4.piimg.com/567571/6ca5c01e9cbfebf7.png)
	

	- 注册好了，登录digitalocean ，此时会要求关联卡，或者关联penpal账号，按照提示的操作就可以。  paypal就是国外的支付宝哈。卡的话普通银联卡好像还不行，信用卡标有mastercard,或visa字样的就可以，国外的就用这些。
	
	- 账号创建好了，就开始生成第一台自己的云主机。

	- 点击主页的 create Droplet. 镜像选择ubuntu16.04， 14.04版本 都可以，这两个用的人多，google问题也不会那么冷门。
	

	- 大小的话，选择最小的就可以了，反正是个人使用。 $5/mon 就是30圆一个月哈，地区的话选择San Francisio, 其他的网络不怎么好，卡卡的，前提的你在innerchina哈。 
		
	![image](http://i4.piimg.com/567571/14167b9d894f95db.png)

	- 在下来的ssh key如果你是IT控，经常要耍这台云主机的话就可以加一个ssh key 不然就不用，作用的不用每次登入云主机都需要密码。
	
	![](http://i4.piimg.com/567571/10a109919f55ddad.png)

	- 其他不用改，操作完了直接点击create，一台云主机就这么诞生了。

2.  接下来就开始操作我们的云主机了。

	- 首先我们得先重置登入密码。
	
	![](http://i4.piimg.com/567571/8c3a1635c915fc43.png)

	![](http://i4.piimg.com/567571/b324e819eaf6a851.png)

	- 重置完后，新密码会发送到我们的注册邮箱。

	- 点击console进入控制台。（或者你本地有ssh的话，可以直接通过ssh登录）
	![](http://i4.piimg.com/567571/b324e819eaf6a851.png)

	- 用户名root 密码是你邮箱收到的密码
	
	![](http://i2.piimg.com/567571/a71cc456ad677b8b.png)

	- 登入首次会要求你改密码，先输入原密码，再输入两次新密码。

	- 这样你就出现在自己的ubuntu云主机上了。

3. 接下来到我们的地上最后一步，搭建shadowsocks服务端了，接下来几步，相信玩过ubuntu的都不会陌生，但新手也没关心，照着命令一个一个敲就可以了。

	- 首先安装shadowsocks,三条命令，
	
	`apt-get install pip`
	
	`pip install --upgrade pip`
	
	`pip install shadowsocks`
	
	这样shadows就安装好了。
	
	- 确认安装shadowsocks
	
		`ssserver --version`
	
	- 编写shadowsocks.json配置脚本(把命令中的密码改成想要的密码)。
	
		`touch /etc/shadowsocks.json`
		`echo "{
    'server':'0.0.0.0',
    'server_port':8888,
    'local_address': '127.0.0.1',
    'local_port':1080,
    'password':'youpassword',
    'timeout':300,
    'method':'aes-256-cfb',
    'fast_open': false
}" > /etc/shadowsocks.json`

	- 运行shadowsocks.json
	
	`ssserver -c /etc/shadowsocks.json -d`  	-d表示在后台运行。
	
	这样，shadowsocks服务端就运行成功了。
	
	


	
	
	
		
  

	
	







	 





	