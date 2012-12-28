---
comments: true
date: 2010-10-27 13:09:25
layout: post
slug: xmarks%e4%bd%bf%e7%94%a8%e8%87%aa%e5%bb%ba%e6%9c%8d%e5%8a%a1%e5%99%a8%e6%8e%a5%e4%b8%8a%e7%af%87
title: Xmarks使用自建服务器[接上篇]
wordpress_id: 143
categories:
- 技术
tags:
- FTP
- Xmarks
---

Xmarks的去留又发生了戏剧性的转变，未来可能提供收费服务鸟～虽然我是很支持收费滴,看看银行卡就我那点可怜的工资.我的心里价位5$/yr左右差不多可以接受～但是一大群美帝在xmarks官方blog里纷纷表示10～15/mo，靠着不是赤裸裸的豁胖吗！
经过一番Google，发现可以用ftp空间做服务器。
step1.进xmarks设置。
step2.高级选项卡->勾选使用自建服务器。服务器填上ftp://citycatcher.info/bookmarks/xmarks.json（路径根据ftp路径）.password URL:ftp://citycatcher.info/bookmarks/password.json.用户名和密码不再是xmarks的用户名和密码，而是ftp帐号。
-EOF-
