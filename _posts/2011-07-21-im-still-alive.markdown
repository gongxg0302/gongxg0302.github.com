---
comments: true
date: 2011-07-21 15:08:41
layout: post
slug: im-still-alive
title: I'm still alive~
wordpress_id: 187
categories:
- 未分类
---

最近折腾的东西越来越多了，一来上班比较闲，二来木有妹子来扰乱身心。n周（大概3个礼拜前），即从Ubuntu->Arch的进阶之后完成了从Arch->Gentoo的质的飞跃～^_^并且在1周前装了第二遍，一来巩固，二来精简。总体来说用Gentoo还是蛮爽的。能充分发挥硬件性能，提升运行速度。期间的bug什么的都通过Google先生解决了，就不费述了。（一个是把GNU make降级到3.81还有就是在最后启动前在/dev里加字符设备consolas好象是这么拼来着）
这周在GReader上乱翻（好像是Twitter上～）看到用dns tunnel转发IP包实现强奸天朝三大运营商，遂折腾之。（目前无果）大致原理就是利用DNS的递归查询。将xx.domain.com(不存在的二级域名)的NS记录绑定到一个能被解析的域名如：yy.domain.com最后将yy.domain.com的A记录设置成你的VPS。（自认为理解无误）最后在VPS上运行iodined这个邪恶的程序就行了。可我折腾近一周无果~望折腾过的朋友点拨下～小弟在此<del>泄</del>谢了...
