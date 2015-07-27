Linux image loading from u-boot
================================

If there is some error with your kernel ,that you can not load a image from console using ftpget ,you have another choice.

This document introduce a new and faster way to load image to flash from u-boot.This tutorial is the static ip way.


Download and install Tftpd32 on windows
-------------------------------------

*[Download tftpd32 from here](http://tftpd32.jounin.net/).*

*[Install tftpd32 on windows using this link](http://wiki.emacinc.com/wiki/Installing_TFTP_server).*

Load image using u-boot
-------------------------------

1. Into the U-boot console,type the following command:

        MSE500-Boot> setenv ipaddr 192.168.5.21
        MSE500-Boot> setenv serverip 192.168.5.200

2. Check connectivity using ping:

        MSE500-Boot> ping 192.168.5.200
        Using MAC Address B8:F8:28:33:12:30
        PHY address is 0x05
        IC+ IP175D found.
        host 192.168.5.200 is alive

3. Unprotect flash if necessary:

        
        MSE500-Boot> protect off all
        Un-Protect Flash Bank # 1
        .....................................................................................................................        ........... done


4. Flash the image:

        MSE500-Boot> tftp 0x40100000 linux-kernel-2.6.25.10-arm
        Using MAC Address B8:F8:28:33:12:30
        PHY address is 0x05
        IC+ IP175D found.
        miiphy_register: non unique device name 'synop3504'
        TFTP from server 192.168.5.200; our IP address is 192.168.5.21
        Filename 'linux-kernel-2.6.25.10-arm'.
        Load address: 0x40100000
        Loading: #################################################################
          #################################################################
          #################################################################
           #################################################################
           #################################################################
           #################################################################
          #################################################################
          #################################################################
          #################################################################
          ###################
        done
        Bytes transferred = 3091572 (2f2c74 hex)
        
        
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

        
5. Restart the board:

Userful links
--------------------

*[Loading_Images_with_U-Boot](http://wiki.emacinc.com/wiki/Loading_Images_with_U-Boot).*

