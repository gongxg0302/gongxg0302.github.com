---
comments: true
date: 2012-03-30 13:58:52
layout: post
slug: '%e6%b2%a1%e6%82%ac%e5%bf%b5%ef%bc%8c%e8%b7%9fopenvpn%e8%80%97%e4%b8%8a%e4%ba%86%e5%86%8d%e4%b8%8d%e5%bc%84%e7%82%b9%e4%b8%9c%e8%a5%bf%ef%bc%8c%e8%bf%99%e5%84%bf%e5%b0%b1%e5%bd%bb%e5%ba%95%e8%8d%92'
title: 没悬念，跟OpenVPN耗上了~(再不弄点东西，这儿就彻底荒废了
wordpress_id: 234
categories:
- 技术
post_format:
- Standard
tags:
- Linux
- VPN
---

和[上次](http://blog.izheteng.us/?p=209)的不一样，这次OpenVPN在OpenWRT上做服务端。服务端有两种模式第一种就是在VPS上跑的模式（具体叫什么答不上来了）Tun/Tap设备是一个单独的子网。第二种叫桥接模式。就是tap设备与路由器上的lan口绑定，成为内网的一部分（用brctl show在Openwrt上看，eth0.X、Wlan0是绑在一起的。新接口是br-lan）。
通俗的说相当于在任何能连接Internet的地方，有一根直接通向你路由器内网的网线。解释完，上配置文件！
/etc/config/openvpn

    
     
    config openvpn lan
            option enable 1
            option port 8081
            option proto tcp
            option dev tap
            option ifconfig_pool_persist /tmp/ipp.txt
            option ca /etc/openvpn/server_keys/ca.crt
            option cert /etc/openvpn/server_keys/server.crt
            option key /etc/openvpn/server_keys/server.key
            option dh /etc/openvpn/server_keys/dh1024.pem
            option comp_lzo 1
            option persist_key 1
            option persist_tun 1
            option status /tmp/openvpn-status.log
            option verb 3
            option server_bridge "192.168.1.1 255.255.255.0 192.168.1.200 192.168.1.219"
            option client_to_client 1
            list push "redirect-gateway"
            list push "dhcp-option DNS 192.168.1.1"
            option mssfix 1400
    


在/etc/config/network里面修改


    
     
    config 'interface' 'lan'
            option 'type' 'bridge'
            option 'proto' 'static'
            option 'ipaddr' '192.168.1.1'
            option 'netmask' '255.255.255.0'
            option 'ifname' 'eth1 tap0' <<------------- add tap0 to lan to create the bridge
    


在/etc/config/firewall最后加上

    
     
    config rule
            option target           ACCEPT
            option dest_port        [PORT]
            option src              wan
            option proto            tcpudp
            option family           ipv4
    


让外网能访问OpenVPN端口。
至于keys的话，可以在Openvpn下通过pkg install openvpn-easy-rsa生成，也可以在VPS上Linux PC上生成。
最后客户端配置文件借用下Openwrt wiki的

    
     
    client
    remote robrobinette.com 1194 # my website and port 1194 (standard port for OpenVpn)
    proto udp
    dev tap
    nobind
    ca ca.crt
    cert client1.crt
    key client1.key
    comp-lzo
    verb 3
    keepalive 10 120
    resolv-retry infinite
    mute-replay-warnings
    mute 20
    

请自行修改。
记得/etc/init.d/network restart && /etc/init.d/firewall restart
在启动脚本中加入

    
     
    iptables -I FORWARD -o br-lan -j ACCEPT
    iptables -I FORWARD -o tap0 -j ACCEPT
    iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -j MASQUERADE
    

允许client访问Internet.
打完，收工。
