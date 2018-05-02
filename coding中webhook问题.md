<!--
title: Coding持续部署中webhook问题
author: 板栗
category: tech
tags: webhook
-->

> coding中有个webhook的功能，通过这个webhook，可以实现产品持续交付的过程。具体就是开发环境中push一次，webhook就会去触发生产环境的同步代码的脚本，就实现了线上代码的自动更新。

**背景：**

- webhook的回调地址类似这样：  `http://hostname/webhook.php`

- 这个脚本webhook.php就在项目的根目录,同时我们也加一个webhook.log用于记录更新日志。 


**问题：**

 1. 一开始的时候我直接写的脚本webhook是这样子的：
 
 	```
 	<?php
 	shell_exec('pwd >> /service/gitblog/webhook.log');
 	$date = date('Y-m-d H:i:s');
 	shell_exec("echo ' {$date} 更新完成。' >> /service/gitblog/webhook.log ");

 	```
 
 	脚本很简单，就三行，原理很简单，运行的时候发现脚本执行完了，本地仓库却没有更新。
 	
 2. 一通google之后，再去检查了coding的配置，发现好像存在一个问题，就是webhook触发的时间是在仓库push操作的一开始，所以，这个时候仓库中的代码是正在更新的状态，还不是最新。 这个时候如果就触发同步脚本,线上的代码是不会更新的。所以就有了接下来这个版本的webhook.php :
 
 	```
 	<?php
 	shell_exec("echo '等待PUSH完成' >> /service/gitblog/webhook.log ");
 	shell_exec('sleep 10');
 	shell_exec("echo '开始更新。' >> /service/gitblog/webhook.log ");
 	shell_exec('pwd >> /service/gitblog/webhook.log');
 	$date = date('Y-m-d H:i:s');
 	shell_exec("echo ' {$date} 更新完成。' >> /service/gitblog/webhook.log ");
 	
 	```
 	
 	原理很简单，既然还没push完成，那我就等10s再开始pull. 然而发现还是没有更新。这个时候发现一个问题，那就是如果我手动在shell中运行webhook.php的时候，其实是可以pull到新的修改的，然而用webhook触发的时候就不会更新。
 
 3. 继续google, 查了好一阵子，找到了问题。手动执行用户是root,webhook触发的用户是www-data。 也就是说www-data是没有git pull的权限的，到了这里终于想到了问题的根本原因，然后开始寻找解决办法。 google到一个，用sudo的方式去执行git pull ; 
	
**最终解决方案：**  

	```
	<?php
	shell_exec('echo "线上更新中。。。" >> /service/gitblog/webhook.log');
	shell_exec('echo "等待push完成 " >> /service/gitblog/webhook.log');
	shell_exec('sleep 5');
	
	shell_exec('echo "执行git pull : " >> /service/gitblog/webhook.log');
	shell_exec('cd /service/gitblog');
	shell_exec('sudo -u root -S git pull origin master < /.passwd >> /service/gitblog/webhook.log 2>&1');
	shell_exec("echo '更新完成,删除缓存中。。。' >> /service/gitblog/webhook.log ");
	
	$date = date('Y-m-d H:i:s');
	shell_exec('rm -rf /service/gitblog/app/cache/*');
	shell_exec("echo '缓存删除完成。' >> /service/gitblog/webhook.log ");
	shell_exec("echo '===================== {$date} ======================' >> /service/gitblog/webhook.log ");

	```

其中有几点需要注意一下：

- .passwd 这个文件里就一个密码，是www-data用户的密码。 [详细说明](http://stackoverflow.com/questions/3173201/sudo-in-php-exec)

- www-data 这个用户是没有sudo的权限的，需要在/etc/sudoers这个文件里添加。 [详细说明](http://www.linuxidc.com/Linux/2010-12/30386.htm)

- 然后因为这个是gitblog的一个部署，所以直接更新完成后再删一下缓存。 Perfect !


到这里这个问题就解决了。

   	


	
	
	
		
  

	
	







	 





	