---
comments: true
date: 2010-03-23 14:59:04
layout: post
slug: '%e5%9c%a8ubuntudebian%e4%b8%8b%e6%90%ad%e5%bb%bavpn%e6%9c%8d%e5%8a%a1%e5%99%a8%ef%bc%88%e5%af%b9%e6%8a%80%e6%9c%af%e6%97%a0%e7%88%b1%e7%9a%84%e6%85%8e%e5%85%a5%ef%bc%89'
title: 在Ubuntu/Debian下搭建VPN服务器（对技术无爱的慎入）
wordpress_id: 39
categories:
- 未分类
---

建站以来所有的文章都是发发牢骚、骂骂档之类的。还没写过一篇技术类的，本来买好VPS就想把从买域名到搭WordPress、VPN的整个过程都写出来的，无奈事情太多，生活中变数太多。最后，有人看也好，没人看也好，希望被Google的爬虫爬到可以方便更多的人。
1.第一步当然是apt-get啦

    
    
    apt-get install pptpd
    


2.配置/etc/pptpd.conf
   反注释添加如下条目：（推荐用VIM编辑器）

    
    
    option /etc/ppp/pptpd-options
    localip 10.0.0.1
    remoteip 10.0.0.234-238,10.0.0.245
    


3.配置/etc/ppp/pptpd-option
    同上，还是把某些行反注释：

    
    
    name pptpd
    refuse-pap
    refuse-chap
    refuse-mschap
    require-mschap-v2
    require-mppe-128
    ms-dns 208.67.222.222
    ms-dns 208.67.220.220
    proxyarp
    nobsdcomp
    lock
    


4.用户名密码。编辑/etc/ppp/chap-secrets
        比如
    
    
    gxg pptpd abc *


        用户名:gxg。密码:abc
5.配置IP Forward
   
    
    
    echo 1 > /proc/sys/net/ipv4/ip_forward
    


6.最后配置Iptables:
   　
    
    
    iptables -t nat -A POSTROUTING -s 10.0.0.0/255.255.255.0 -o eth0 -j MASQUERADE
    



PS.完成了，时间仓促。有错再改。


