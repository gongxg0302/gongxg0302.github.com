---
comments: true
date: 2010-11-14 15:01:07
layout: post
slug: guizmovpn%e6%8a%98%e8%85%be%e8%ae%b0%ef%bd%9e
title: guizmovpn折腾记～
wordpress_id: 145
categories:
- 技术
tags:
- iPhone
- VPN
- 苹果
---

其实呢，自从我的iPhone越狱后我的手就有一点欠。话说周五晚上和老妈百无聊赖的坐在沙发上，手里摆弄着iPhone，看到小i也能facetime了。我不淡定了～装了"faceIt"这个程序，重启，没反应？！卸载，再重装。结果，悲剧帝诞生了。iPhone那脆弱的BSD系统崩溃了。也就是启动不了，一直白苹果。
好吧，既然事以至此，我也不愿老天了。刷机、jailbreak、装七装八的。（省略500+字）
入正题，按照我的理解。OpenVPN就是OpenSSL+tun设备实现的一个加密的VPN隧道。虽然底层都是有开源项目构成的，但是。
我想没人希望通过Terminal打一堆七七八八命令用上OpenVPN吧。所以，有人吧这些七七八八的东西做成一个gui封装的易用软件；那么他就能心安理得的定个价了。只可惜作者胃口大了。9.99$...经过一番Google !@#$%.....
GuizmOVPN.v1.0.1.iPhone.iPod.Touch.Incl.Keymaker-COREPDA 嘘～
接下来正式开始安装。先去喝口水，瞄一眼F1......
首先，不管怎样连到乃的iPhone，看到root@iPhone:~#
这里我要声明，我爱Debian，除了那巨恶心的package dependce~
但是还是要面对滴~
确认有没有apt-get命令，没有的话去apt.saurik.com下apt_0.7.20.2-21_iphoneos-arm.deb & berkeleydb_4.6.21-4_iphoneos-arm.deb原因是前者依赖后者。
继续，apt-get install network-cmds libpcap libstatusbar
最后一步，dpkg -i ./guizmovpn.XXXXX
然后respring一下，就能看到guizmovpn的图标了。
就剩配置了，把ca.crt client1.crt client1.key放在某个文件夹下打包成.zip
接下来在iPhone上打开guizmovpn的web server选项。电脑访问http://[iPhone IP]:2095（端口号不定）
sumbit .zip
It works!

