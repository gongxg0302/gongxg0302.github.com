---
comments: true
date: 2010-03-24 13:56:25
layout: post
slug: '%e5%9c%a8ubuntudebian%e4%b8%8b%e6%90%ad%e5%bb%bavpn%e7%ac%ac%e4%ba%8c%e5%ad%a3openvpn'
title: 在Ubuntu/Debian下搭建VPN第二季(OpenVPN)
wordpress_id: 46
categories:
- 未分类
---

回顾一下，上一季介绍了在Ubuntu下搭建了基于PPTP的VPN.但是在不同的网络环境下基于PPTP的VPN连接后会有各种各样的问题，比如在我机器上DNS Forwarding会有问题，某些内容还是会被GFW干掉。刚才用了10分钟配置了一下OpenVPN。发觉OpenVPN也不会比pptpd麻烦多少。以下是过程：

1.第一步依旧是apt-get大法
        
    
    
    apt-get install openvpn



2.从/usr/share/doc中copy配置文件到OpenVPN目录中
         
    
    
    cp -R /usr/share/doc/openvpn/examples/easy-rsa /etc/openvpn
    



3.建立客户端/服务器证书
          
    
    
    . ./vars
    ./clean-all
    ./build-ca
    ./build-key-server server
    ./build-key client1
    ./build-dh
    



4.配置iptables
           
    
    
    iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j SNAT --to [Server IP]
    



5.配置IP转发
           
    
    
    /sbin/sysctl -w net.ipv4.ip_forward=1
    echo 1 > /proc/sys/net/ipv4/ip_forward
    



6.在/etc/openvpn/下建立openvpn.conf

    
        dev tun
        proto tcp
        port 1194
    
        ca /etc/openvpn/easy-rsa/2.0/keys/ca.crt
        cert /etc/openvpn/easy-rsa/2.0/keys/server.crt
        key /etc/openvpn/easy-rsa/2.0/keys/server.key
        dh /etc/openvpn/easy-rsa/2.0/keys/dh1024.pem
    
        user nobody
        group nogroup
        server 10.8.0.0 255.255.255.0
    
        persist-key
        persist-tun
    
        #status openvpn-status.log
        #verb 3
        client-to-client
    
        push "redirect-gateway"
        push "dhcp-option DNS 8.8.8.8"
        push "dhcp-option DNS 8.8.4.4"
    
        comp-lzo



7.从服务器上copy证书&Windows;下配置
        （1）包括ca.crt&client1.crt;&client.key;
        （2）安装openvpn,建立client.opvn添加如下内容

    
    
    client
    dev tun
    proto tcp
    
    # The hostname/IP and port of the server.
    # CHANGE THIS TO YOUR VPS IP ADDRESS
    remote [Your Server IP] 1194
    
    resolv-retry infinite
    nobind
    
    persist-key
    persist-tun
    
    ca ca.crt
    cert client1.crt
    key client1.key
    
    comp-lzo 
    verb 
    


        
8.完成可以连接了。
        
    
    
    /etc/init.d/openvpn start
    
