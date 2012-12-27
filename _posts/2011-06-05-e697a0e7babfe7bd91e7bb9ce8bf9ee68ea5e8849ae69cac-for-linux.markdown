---
comments: false
date: 2011-06-05 14:18:14
layout: post
slug: '%e6%97%a0%e7%ba%bf%e7%bd%91%e7%bb%9c%e8%bf%9e%e6%8e%a5%e8%84%9a%e6%9c%ac-for-linux'
title: 无线网络连接脚本 for linux
wordpress_id: 182
categories:
- 技术
---

在S30上面装上Arch Linux已经很久了～（内核都升级了好几次了）由于ThinkPad S30用的那无线网卡是Prism 2，嗯～比较少见的卡。modprode orainoco_cs后系统可以识别网卡（也就是iwconfig有信息输出），但是进X-org后nm-applet没法连接无线网络（用iwconfig手动可以连接），具体原因看完log后我也一头雾水。算了还是写个脚本～

    
    
    #!/bin/bash
    interface=eth0
    essid=Openwrt
    key=ABCDEF0302
    ifconfig $interface up
    sleep 2
    while [ "$(ifconfig ${interface} |grep "inet addr"| cut -f 2 -d ":"|cut -f 1 -d " ")" = '' ]
    do
    	iwconfig $interface essid $essid key $key
    	sleep 5
    	dhcpcd eth0
    	sleep 10
    done
    
