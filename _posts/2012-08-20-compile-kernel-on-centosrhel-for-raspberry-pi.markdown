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
     直接下载crosstool-ng的最新版。解压后

    
    
    ./configure --prefix=[目录]
    


然后记得把目录加进$PATH

    
    
    PATH=[目录]/bin:"$PATH"
    


运行下面命令前先确定当前目录至少有4GB左右的空闲空间

    
    
    ct-ng menuconfig
    


到“Paths and misc options”选上“Try features marked as EXPERIMENTAL.”
“Prefix directory”什么的最好别改，如果真要改记得加“${CT_TARGET}”否则你就蛋疼吧。
回主菜单，“Target architecture”改成arm，别的什么“Little endian”和“Bitness set to 32bit”应该都不用改。
回主菜单，“Target OS”选linux.
回主菜单，"Binary utilities"选不带EXPERIMENTAL的最新版。“gcc version”我是随便选的。2012-06的。
然后退出，保存。
在开始编译工具链前设置下$PATH变量为你的[Prefix directory]/bin:

    
    
    ct-ng build
    


如果不出意外的话运行

    
    
    arm-unknown-linux-gnueabi-gcc --version
    


之后找个顺眼的目录下kernel源码和Rpi的工具

    
    
    git clone https://github.com/raspberrypi/tools.git
    git clone https://github.com/raspberrypi/linux.git
    cd linux
    cp arch/arm/configs/bcmrpi_cutdown_defconfig .config
    


可以编辑加上一些选项，不过我加上iotop缺少的选项好像没用。估计是源码里没有。当然，你可以clone非官方内核。
之后

    
    
    make ARCH=arm CROSS_COMPILE=/usr/bin/arm-linux-gnueabi- menuconfig
    make ARCH=arm CROSS_COMPILE=/usr/bin/arm-linux-gnueabi- -k -j[CPU数量加1]//在别人机器上用5真不心疼，haha~
    


当然oldconfig也行。
编译完后建个目录比如../modules

    
    
    make modules_install ARCH=arm CROSS_COMPILE=/usr/bin/arm-linux-gnueabi- INSTALL_MOD_PATH=../modules/
    cd ../tools/mkimage/
    ./imagetool-uncompressed.py ../../linux/arch/arm/boot/Image
    


以上命令将内核模块安装在../modules,生成kernel.img文件。
最后把../modules里的/lib/modules&/lib/firmware替换Rpi里相同目录，kernel.img拷到SD卡第一个FAT32分区就行了。
  
  本文参考了：  
  [http://blog.saladlam.info/index.php/Notepad/?p=169&more;=1&c;=1&tb;=1&pb;=1](http://blog.saladlam.info/index.php/Notepad/?p=169&more=1&c=1&tb=1&pb=1)  
  [http://mitchtech.net/raspberry-pi-kernel-compile/](http://mitchtech.net/raspberry-pi-kernel-compile/)  
  [http://www.bootc.net/](http://www.bootc.net/)
