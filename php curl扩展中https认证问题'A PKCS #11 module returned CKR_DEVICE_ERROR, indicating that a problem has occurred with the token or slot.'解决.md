<!--
author: 板栗
head: 
date: 2017-02-19
title: php curl扩展中https认证问题'A PKCS #11 module returned CKR_DEVICE_ERROR, indicating that a problem has occurred with the token or slot.'解决
tags: PHP
images: 
category: PHP
status: publish
summary: 
-->

**环境：**  

centOS 7.2

**问题：**

在PHP使用curl请求https的链接的时候，出现错误

```
A PKCS #11 module returned CKR_DEVICE_ERROR, indicating that a problem has occurred with the token or slot.
```

**原因：**

centOS7.2 等某些系列版本中，curl更新后变成使用的是nss代替openssl进行相关的操作造成了此问题。 

redhat 官方给出了相关的解释(Gary使用yum update重现了过程，并说明了原因)

[cURL gets CKR_DEVICE_ERROR when posting over SSL since yum update](https://bugzilla.redhat.com/show_bug.cgi?id=870856)

**解决办法：**

1. 从[curl官网](https://curl.haxx.se/download.html)中找到最新的curl，下载到服务器，重新编译安装curl , 编译参数：

	```
	# ./configure --without-nss --with-ssl  

	# make

	# make install
	```

2. 把curl编译好的lib添加到系统的PATH中

	```
echo "/usr/local/lib" >> /etc/ld.so.conf && ldconfig   //这个 '/usr/local/lib' 也有可能是其他目录，请观察make编译到哪个目录里，没错，就是那个lib目录，路径你自己看。 
	```

3. 重新编译php吧。 或者如果你可以重新编译php里面的curl扩展，那也是没有问题的。 


参考：

[将centos6的curl ssl版本(NSS)替换成openssl,解决Unable to load client key -8178的错误问题  ](http://nznchong.blog.163.com/blog/static/25398492014394279615/)


