<!--
title:  phpstorm中sftp同步文件缓慢解决
author: 板栗
category: tech
tags: webhook
-->

> phpStorm是目前比较火的一个IDE，然后其中的功能丰富，经常我们用到的只是其中的一丢丢，这里我就来说说phpStorm中SFTP的一个问题。 

**背景：**
开发环境是我的电脑用phpStorm的的SFTP功能直接连的开发机然后开始编程。怎么用SFTP连，如果需要教程的话，在评论中说一下，我就更新上去。


**问题：**

 自己有一台美国的服务器，SFTP连接缓慢，配置完成后会开始同步远程服务器的目录到本地，这个时候如果文件稍微多一些就有的等了，没招，开始搞phpStorm。
	
**解决方案：**  

   - 在SFTP的配置完成的最后一步，是选择项目的根路径，同时，这个时候我们将整个根路径设置成排除路径(Exclude Path), 这个时候点完成，就不会去一直同步远程服务器的文件了。
   
   
   	![](http://o8adahup8.bkt.clouddn.com/phpstorm2.png?imageView/2/w/500/h/450)
   	
  
  - 完成后，你会发现这个时候Remote Host目录里的文件一个也下载不下来了，因为跟路径都被我们置为Exclude Path了。所以修改一下就好了。 导航栏Tools->configuration->编辑你的远程服务器->到Excluded Paths目录下，将其中唯一的选项 / 移除（remove path） 就可以了
  
 	 ![](http://o8adahup8.bkt.clouddn.com/phpstorm1.png?imageView/2/500/h/450)
   
   - 开发的时候，我们从侧边栏的remote host中的同步所需要的文件就可以了。比如说要修改某个文件，只要将那个文件下载下来就可以了。
 
 > 如果你正在找phpStorm的快速入门，这里推荐个视频，看完后phpStorm勉强可以用的6一些了。哈哈！[点这里](https://laravist.com/series/phpstorm-the-best-php-ide-you-ever-met)   
 


	
	
	
		
  

	
	







	 





	