<!--
author: banli 
head: 
date: 2017-04-30
title:  frp排坑
tags: tech
images: 
category: tech 
status: publish
summary:  
-->

---

> 写在前面： 
仓库地址:https://github.com/fatedier/frp
A fast reverse proxy to help you expose a local server behind a NAT or firewall to the internet.

QA:

- 问题:  映射web服务的时候，用ip无法访问 
- 解决:  映射web只能通过域名访问

ssh -oPort=6000 test@x.x.x.x 
    

