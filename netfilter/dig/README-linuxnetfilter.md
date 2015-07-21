Linux netfilter digging 
================================

在简单理解linux netfilter的原理之后，接下来会深入挖掘其中的原理。

Linux packet handling process
-------------------------

[Packet handling process when climb out of the driver layer](http://open-source.arkoon.net/kernel/kernel_net.png)
[Comprehensive explanation on TCP/IP stack](http://samples.sainsburysebooks.co.uk/9780470377840_sample_382501.pdf)

MSE500添加netfilter的步骤
-------------------------------

1. 在下面的路径新建hanftest文件夹:

        wu@ubuntu:~/mse500-0.9.4/linux-2.6.25.10-spc300/net$ pwd
        /home/wu/mse500-0.9.4/linux-2.6.25.10-spc300/net
        wu@ubuntu:~/mse500-0.9.4/linux-2.6.25.10-spc300/net$ mkdir hanftest
        wu@ubuntu:~/mse500-0.9.4/linux-2.6.25.10-spc300/net$ cd hanftest/

2. 创建netfilter主文件，Kconfig文件及Makefile:

        wu@ubuntu:~/mse500-0.9.4/linux-2.6.25.10-spc300/net/hanftest$ vim hanftest.h
        wu@ubuntu:~/mse500-0.9.4/linux-2.6.25.10-spc300/net/hanftest$ vim hanftest.c
        wu@ubuntu:~/mse500-0.9.4/linux-2.6.25.10-spc300/net/hanftest$ vim Kconfig
        wu@ubuntu:~/mse500-0.9.4/linux-2.6.25.10-spc300/net/hanftest$ vim Makefile
 
3. 分别添加下面链接的内容到各个文件:

        hanftest.h is at(https://github.com/wufengyi/homeaccess-learning-doc/blob/master/netfilter/linux-kernel/net/netfilter/hanftest.h).

        hanftest.c is at(https://github.com/wufengyi/homeaccess-learning-doc/blob/master/netfilter/linux-kernel/net/netfilter/hanftest.c).

        Kconfig is at(https://github.com/wufengyi/homeaccess-learning-doc/blob/master/netfilter/linux-kernel/net/netfilter/Kconfig).

        Makefile is at(https://github.com/wufengyi/homeaccess-learning-doc/blob/master/netfilter/linux-kernel/net/netfilter/Makefile).

4. 打开kernel/net下面的Kconfig和Makefile:

        wu@ubuntu:~/mse500-0.9.4/linux-2.6.25.10-spc300/net$ pwd
        /home/wu/mse500-0.9.4/linux-2.6.25.10-spc300/net
        wu@ubuntu:~/mse500-0.9.4/linux-2.6.25.10-spc300/net$ vim Kconfig
        wu@ubuntu:~/mse500-0.9.4/linux-2.6.25.10-spc300/net$ vim Makefile
        
5. 修改Kconfig和Makefile，下面链接的文件是修改后的:

        Here is the new Kconfig(https://github.com/wufengyi/homeaccess-learning-doc/blob/master/netfilter/linux-kernel/net/Kconfig).
        Here is the new Makefile(https://github.com/wufengyi/homeaccess-learning-doc/blob/master/netfilter/linux-kernel/net/Makefile).

配置编译应用程序
------------

1. 进入buildroot下，配置kernel:

        wu@ubuntu:~/mse500-0.9.4$ cd buildroot
        wu@ubuntu:~/mse500-0.9.4/buildroot$ make linux26-menuconfig
        
2. 勾选netfilter和hanftest程序:

   进入Networking/Networking options/，
   选中Network packet filtering framework 和Homeaccess Netfilter Test程序。
   
3. 编译pkg文件:

        wu@ubuntu:~/mse500-0.9.4$ ./buildall.sh mkimg

测试应用程序
------------

1. 把pkg用ftp烧到目标机上:

2. 在Windows上telnet目标机IP:

3. 出现如下LOG证明测试成功:
 
        [TCP_C_IN] DEBUG: From IP address: 192.168.5.200
        [TCP_C_OUT] DEBUG: To IP address: 192.168.5.200
        [TCP_C_IN] DEBUG: From IP address: 192.168.5.200
        [TCP_C_OUT] DEBUG: To IP address: 192.168.5.200
        [TCP_C_IN] DEBUG: From IP address: 192.168.5.200
        [TCP_C_OUT] DEBUG: To IP address: 192.168.5.200
        [TCP_C_IN] DEBUG: From IP address: 192.168.5.200
        [TCP_C_OUT] DEBUG: To IP address: 192.168.5.200


有用的资源
--------------------

*[Understand the linux netfilter](https://www.csh.rit.edu/~mattw/proj/nf/).*

*[Adding-a-new-kernel-module-to-linux-source-tree](https://geekwentfreak-raviteja.rhcloud.com/blog/2010/10/24/adding-a-new-kernel-module-to-linux-source-tree/?utm_content=buffer03878&utm_medium=social&utm_source=twitter.com&utm_campaign=buffer).*

*[Sample linux netfilter](https://github.com/andrewstucki/netfilter-skeleton).*

*[Sample linux firewall using netfilter](https://github.com/smallen3/Linux-Firewall).*



