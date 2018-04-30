<!--
author: banli 
head: 
date: 2017-04-30
title:  stf 自用填坑指南
tags: tech
images: 
category: tech 
status: publish
summary:  
-->

---

> 写在前面： 
STF（或智能手机测试场）是一款网络应用程序，用于从浏览器中远程调试智能手机，智能手表和其他小工具。
STF安装的时候需要安装各种各样的依赖，所以，我们直接上docker，省去依赖报错的处理过程。


docker-compose.yml 配置如下:

```
rethinkdb:
  image: rethinkdb:2.3
  ports:
    - "8080:8080"
    - "28015:28015"
    - "29015:29015"
  restart: always
  volumes:
    - "/srv/rethinkdb:/data"
  command: "rethinkdb --bind all --cache-size 2048"

adbd:
  image: sorccu/adb
  privileged: true
  ports:
    - "5037:5037"
  restart: always
  volumes:
    - "/dev/bus/usb:/dev/bus/usb"

stf-local:
  image: openstf/stf
  links:
    - rethinkdb
    - adbd
  ports:
    - "7100:7100"
    - "7110:7110"
    - "7120:7120"
    - "7400-7500:7400-7500"
  restart: always
  command: stf local --public-ip 192.168.199.141 --provider-min-port 7400 --provider-max-port 7500 --adb-host adbd
```



QA:

- 问题: 打开网页后，显示不了字。

- 解决: 
    - 打开控制台。css里的font-family 里的lato去掉。
    - 设置里面选择字体为中文就可以了。 
    


---
>  1. 参考教程：  https://github.com/openstf/stf

