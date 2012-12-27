---
comments: true
date: 2011-07-27 14:42:48
layout: post
slug: ip-over-dns-how-to
title: IP over DNS How to~
wordpress_id: 190
categories:
- 未分类
---

前几天在Google Reader上看到的，当时没多想，看完就放一边了。某日上班，百无聊赖之际想起公司的wifi热点故起邪念～
正文：
    首先按照[http://code.kryo.se/iodine/](http://code.kryo.se/iodine/)里的README设置DNS。我用的freedns的subdomain~
   [![dns-setting1_0](http://blog.izheteng.us/wp-content/gallery/blog/thumbs/thumbs_dns-setting1_0.jpg)](http://blog.izheteng.us/wp-content/gallery/blog/dns-setting1_0.jpg)
   说白了就是弄一个子域名添加A记录到你的VPS，然后在随便起个子域名添加NS记录为之前的子域名。本来想用自己的域名的，无奈在Godaddy面板试了半天iodine的check-it总是提示无法在域名服务器上找到tunnel.XXX.info的代理。所以本文用freedns的。
    VPS上没安装OpenVPN的话
    
    modprobe tun
    mknod /dev/net/tun c 10 200


然后运行服务端 
     
    
     
    iodined -c -f 10.0.0.1 -P [password] tunnel.jumpingcrab.com
         


    之后等上个两三天去[code.kryo.se/iodine/check-it](http://code.kryo.se/iodine/check-it)显示well done基本上就成功一大半了。
    后面在客户端运行 
      
    
    
    iodine -f -P password tunnel.jumpingcrab.com
          


如果可以ping通10.0.0.1就说明成功了！接下来用plink或者Tunnelier等ssh隧道就能上网了。上图吧～
[![done](http://blog.izheteng.us/wp-content/gallery/blog/thumbs/thumbs_done.jpg)](http://blog.izheteng.us/wp-content/gallery/blog/done.jpg)



