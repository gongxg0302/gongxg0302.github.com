---
comments: true
date: 2012-08-20 15:00:51
layout: post
slug: compile-kernel-on-centosrhel-for-raspberry-pi
title: Compile Kernel on CentOS/RHEL for Raspberry Pi
wordpress_id: 241
categories:
- 技术
post_format:
- Standard
tags:
- kernel
- Linux
- raspberry pi
---

话说从年初听到Raspberry Pi到上个月正式拿到板子都快半年了。这半年我等的热情都快耗没了。到手以后买SD卡，电源、HDMI线什么的。期间折腾也只限于别人做好的镜像，我只是例行公事的dd进去。
     先是OpenELEC.找了张1GB的旧SD卡，用HDMI接上32”的液晶电视，看720p还很流畅。不过由于电视机分辨率只有1366×768会超出屏幕，也没法改。之后就用Raspbian了，就和用一般的发行版一样了。由于贪便宜入了棒子的8GB class6卡，apt-get速度有点慢。并且考虑了寿命问题，就把rootfs一到了移动硬盘上。上周装了iotop，没法运行。提示有些内核编译选项没有编译进内核，遂折腾之。
     至于本文标题的系统，选择红帽系实在是不得已。谁叫托管在机房的服务器不是我的呢... 
