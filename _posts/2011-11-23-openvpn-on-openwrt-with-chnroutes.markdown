---
comments: true
date: 2011-11-23 13:18:10
layout: post
slug: openvpn-on-openwrt-with-chnroutes
title: OpenVPN on OpenWRT with Chnroutes.
wordpress_id: 209
categories:
- 未分类
---

最近折腾了很多东西，好久没往blog上记了。时间长了怕忘记。、
从标题开始。第一样不解释，大家都懂得。OpenWRT——历史最悠久的开源路由器固件。Chnroutes是Google Code上托管的一个项目，致力于在上网的同时忽略GFW的干扰。说白了就是配合*VPN+静态路由实现有选择的路由。
您需要：支持OpenWRT路由器一只，技术宅一个。
准备工作，OpenWRT安装ip、kmod-tun随后opkg install openvpn。建立/etc/openvpn目录。为防止DNS投毒，在除/tmp以外位置建立resovle.conf,内容nameserver [DNS IP]。最好填三个，以免被dnsmasq用本地DNS补上。编辑/etc/config/dhcp修改option resolvfile       '/etc/openvpn/resolv.conf'. ca.srt client.srt client.key 外加一配置文件（随后给出）。生成up和down脚本，目的是在连接后添加路由记录和断开前清空刚才添加的路由记录。


    
     
    #!/usr/bin/env python
    
    import re
    import urllib
    
    VPNUPBASE="""#!/bin/sh
    export PATH="/bin:/sbin:/usr/sbin:/usr/bin"
    
    OLDGW=`ip route show | grep 'ppp0.*src' | awk '{print $1}'`
    
    if [ $OLDGW == '' ]; then
        exit 0
    fi
    
    if [ ! -e /tmp/openvpn_oldgw ]; then
        echo $OLDGW > /tmp/openvpn_oldgw
    fi
    
    """
    
    VPNDOWNBASE="""#!/bin/sh
    export PATH="/bin:/sbin:/usr/sbin:/usr/bin"
    
    OLDGW=`cat /tmp/openvpn_oldgw`
    
    """
    
    url=r'http://ftp.apnic.net/apnic/dbase/data/country-ipv4.lst'
    
    handler=urllib.urlopen(url)
    
    upfile=open('vpnup','w')
    downfile=open('vpndown','w')
    
    upfile.write(VPNUPBASE)
    upfile.write('\n')
    
    downfile.write(VPNDOWNBASE)
    downfile.write('\n')
    
    for line in handler.readlines():
        if line.find(': cn ') < 0: continue
        r=line.split(':')[1]
        r=r.strip()
        ip,mask=r.split('/')
        ip=ip.split('.')
        while len(ip) < 4:
            ip.append('0')
        
        mask=int(mask)
        
        bm='1'*mask+'0'*(32-mask)
        mask="%d.%d.%d.%d"%(int(bm[0:8],2),int(bm[8:16],2),int(bm[16:24],2),int(bm[24:32],2))
    
        upfile.write('route add -net %s netmask %s gw $OLDGW\n'%('.'.join(ip),mask))
        downfile.write('route del -net %s netmask %s\n'%('.'.join(ip),mask))
    
    downfile.write('rm /tmp/openvpn_oldgw\n')
    
    upfile.close()
    downfile.close()
    

找台联网，又装python的机器跑下。
得到的up和down放到/etc/openvpn下，在路由器上执行chmod +x ./up && chmod +x ./down后备用。
在开始正式配置前运行 route -n 观察一下最后一条。既，0.0.0.0(默认路由)的gateway那一列是外网的网关IP还是0.0.0.0，如果是前者那跳过这步后者的话在/etc/rc.local里加上
    
    
    OLDGW=`ip route show | grep 'ppp0.*src' | awk '{print $1}'`
    route del default
    route add -net 0.0.0.0 netmask 0.0.0.0 gw $OLDGW

此处ppp0为pppoe拨号连接。
解释一下上面做的。在我的路由器上拨号后默认网关指向0.0.0.0接口为ppp0，而在大多数Linux机器上比如VPS上网关为下一条的IP地址。如果是0.0.0.0,openvpn无法push默认路由。
然后，就剩下配置了。编辑/etc/config/openvpn
    
    
    config openvpn VPS_config
    
            # Set to 1 to enable this instance:
            option enable 1
    
            # Include OpenVPN configuration
            option config /etc/openvpn/VPS.conf

后面不改。配置文件名随便起。
附上本例VPS.conf
    
    
    client
    dev tun
    proto udp
    
    # The hostname/IP and port of the server.
    # CHANGE THIS TO YOUR VPS IP ADDRESS
    remote [IP] [port]
    resolv-retry infinite
    mssfix 1400
    persist-key
    persist-tun
    
    ca /etc/openvpn/ca.crt
    cert /etc/openvpn/client1.crt
    key /etc/openvpn/client1.key
    
    comp-lzo
    verb 3
    
    route-delay 2
    
    script-security 2
    up /etc/openvpn/up
    down /etc/openvpn/down


应该就行了。最后运行
    
    
    iptables -I FORWARD -o br-lan -j ACCEPT
    iptables -I FORWARD -o tun0 -j ACCEPT
    iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -j MASQUERADE
    

建议加在/etc/openvpn/up中。
连接：/etc/init.d/openvpn start。
