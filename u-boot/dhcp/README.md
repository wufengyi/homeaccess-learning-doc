Linux image loading from u-boot
================================

If there is some error with your kernel ,that you can not load a image from console using ftpget ,you have another choice.

This document introduce a new and faster way to load image to flash from u-boot,both static ip and dhcp.

Download dhcp server on windows
-------------------------------------

*[Download dhcpsrv2.4.4 from here](http://www.dhcpserver.de/cms/).*


install and start dhcp server on windows
-------------------------------------

Extract the dhcpsrv2.4.4 zip folder,and follow the readme.txt or
*[Go to this link](http://www.dhcpserver.de/cms/running_the_server/).* to install and start dhcp server.


Prepare the linux kernel image 
-------------------------------------

Put your file into dhcpsrv2.4.4/wwwroot


Loading image on u-boot console
-------------------------------------

1. Into the U-boot console,type the following command:

        MSE500-Boot> bootp
        
2. Erase flash and copy image:

        MSE500-Boot> erase 0x30140000 0x3049FFFF
        Erasing sector 20 ... ok.
        Erasing sector 21 ... ok.
        Erasing sector 22 ... ok.
        Erasing sector 23 ... ok.
        Erasing sector 24 ... ok.
        Erasing sector 25 ... ok.
        Erasing sector 26 ... ok.
        Erasing sector 27 ... ok.
        Erasing sector 28 ... ok.
        Erasing sector 29 ... ok.
        Erasing sector 30 ... ok.
        Erasing sector 31 ... ok.
        Erasing sector 32 ... ok.
        Erasing sector 33 ... ok.
        Erasing sector 34 ... ok.
        Erasing sector 35 ... ok.
        Erasing sector 36 ... ok.
        Erasing sector 37 ... ok.
        Erasing sector 38 ... ok.
        Erasing sector 39 ... ok.
        Erasing sector 40 ... ok.
        Erasing sector 41 ... ok.
        Erasing sector 42 ... ok.
        Erasing sector 43 ... ok.
        Erasing sector 44 ... ok.
        Erasing sector 45 ... ok.
        Erasing sector 46 ... ok.
        Erasing sector 47 ... ok.
        Erasing sector 48 ... ok.
        Erasing sector 49 ... ok.
        Erasing sector 50 ... ok.
        Erasing sector 51 ... ok.
        Erasing sector 52 ... ok.
        Erasing sector 53 ... ok.
        Erasing sector 54 ... ok.
        Erasing sector 55 ... ok.
        Erasing sector 56 ... ok.
        Erasing sector 57 ... ok.
        Erasing sector 58 ... ok.
        Erasing sector 59 ... ok.
        Erasing sector 60 ... ok.
        Erasing sector 61 ... ok.
        Erasing sector 62 ... ok.
        Erasing sector 63 ... ok.
        Erasing sector 64 ... ok.
        Erasing sector 65 ... ok.
        Erasing sector 66 ... ok.
        Erasing sector 67 ... ok.
        Erasing sector 68 ... ok.
        Erasing sector 69 ... ok.
        Erasing sector 70 ... ok.
        Erasing sector 71 ... ok.
        Erasing sector 72 ... ok.
        Erasing sector 73 ... ok.
        Erased 54 sectors
        
        
        MSE500-Boot> cp.b 0x40100000 0x30140000 0x360000
        Copy to Flash... ........................................................................................................... done

3. Restart the board:
