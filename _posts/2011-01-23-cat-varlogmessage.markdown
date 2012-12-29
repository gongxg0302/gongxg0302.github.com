---
comments: true
date: 2011-01-23 15:30:41
layout: post
slug: cat-varlogmessage
title: cat /var/log/message
wordpress_id: 165
categories:
- 技术
tags:
- Arch Linux
- ThinkPad
---

比写博神马更讨厌的是起标题！
最近一周再折腾Arch Linux。也算对Linux有更进一步了了解，包括每种发行版底层的差异。Arch和其他主流的桌面发行版例如Rathat/Fedora、Ubuntu、openSUSE主要的理念就是arch的高可定制性(LSF、Gentoo勿笑～)。虽然Gentoo也是高度可定制，但我觉得我需要的就是bin的方便和偶尔编译一些新软件，而arch结合了以上两点。作为一个不折腾会死星人，我把我那猥琐的眼神落在经典老机ThinkPad S30上，虽然CPU和RAM都不及iPad，但是我可以充分发挥arch的优势定制一个适合老机器的桌面Linux。
安装过程参考Arch Wiki，直到安装完毕都没有神马意外。（安装在USB设备上需要在/etc/mkinitcpio.conf的"HOOKS="后加上USB）
之后就是安装Xorg这个巨恶心的东西了。安装完Xorg和一堆七七八八的包后startx，如果成功启动X-window。那么，说明你的机器已经比较古董了。如果进不去X，去/var/log/X.0.log看日志找出问题，然后调教xorg.conf。如果是Nvidia&ATI;的卡，直接去装各自的闭源驱动吧～装完后X应该就自动配置好了。
![顺利进X](http://distillery.s3.amazonaws.com/media/2011/01/22/6589b303ff844ae19e22bd52ac4855a4_7.jpg)  
之后是选窗口管理器。哥用的是openbox+tint2(pannel)。都很lightweight~
要在启动X时启动Window-Manager，在～/.xinitrc中加入"exec openbox-session dbus-launch"
要启动面板tint2在~/.config/openbox/autostart.sh中加入tint2&,另外要设背景的话用feh~
Pcman用来管理文件,firefox么，你懂得～Gvim编辑器。就这样，很简单～
最后因为要用Yaourt装一个字体连到sourceforge被墙（应该不是墙，节点停在中华电信）。就用了下VPN，翻了下wiki，就数OpenVPN最简单了。把配置文件放到~/.openvpn下，最后openvpn [配置文件名]就行了～
碎叫～
