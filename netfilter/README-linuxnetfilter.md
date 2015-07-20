Linux netfilter small test
================================

在Linux kernel上添加了一个netfilter的application。
顺便说明一下Linux netfilter的支持与应用，我们一起来学习。

Linux netfilter 简介
-------------------------

linux netfilter提供了在packet经过网关的时候操纵packet的一种机制，根据packet里的原地址和目的地址，kernel能够进行 passed, blocked or redirected to another IP/port
三种操作机制。具体的原理里面涉及到的东西比较多，在这里不赘述。下面的链接讲的比较详细。

*Understand the [linux netfilter](https://www.csh.rit.edu/~mattw/proj/nf/).*

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
 
3. 分别添加内容到各个文件:

        *This is the [hanftest.h](https://github.com/wufengyi/homeaccess-learning-doc/blob/master/netfilter/linux-kernel/net/netfilter/hanftest.h).*

        *This is the [hanftest.c](https://github.com/wufengyi/homeaccess-learning-doc/blob/master/netfilter/linux-kernel/net/netfilter/hanftest.c).*

        *This is the [Kconfig](https://github.com/wufengyi/homeaccess-learning-doc/blob/master/netfilter/linux-kernel/net/netfilter/Kconfig).*

        *This is the [Makefile](https://github.com/wufengyi/homeaccess-learning-doc/blob/master/netfilter/linux-kernel/net/netfilter/Makefile).*

4. 打开kernel/net下面的Kconfig和Makefile:

        wu@ubuntu:~/mse500-0.9.4/linux-2.6.25.10-spc300/net$ pwd
        /home/wu/mse500-0.9.4/linux-2.6.25.10-spc300/net
        wu@ubuntu:~/mse500-0.9.4/linux-2.6.25.10-spc300/net$ vim Kconfig
        wu@ubuntu:~/mse500-0.9.4/linux-2.6.25.10-spc300/net$ vim Makefile
        
5. 修改Kconfig和Makefile:

        *This is the new [Kconfig](https://github.com/wufengyi/homeaccess-learning-doc/blob/master/netfilter/linux-kernel/net/Kconfig).*
        *This is the new [Makefile](https://github.com/wufengyi/homeaccess-learning-doc/blob/master/netfilter/linux-kernel/net/Makefile).*

配置编译应用程序
------------

1. 进入buildroot下，配置kernel:

        wu@ubuntu:~/mse500-0.9.4$ cd buildroot
        wu@ubuntu:~/mse500-0.9.4/buildroot$ make linux26-menuconfig
        
2. 勾选netfilter和hanftest程序:

   进入Networking/Networking options/
   选中Network packet filtering framework 和Homeaccess Netfilter Test程序。
   
3. 编译pkg文件:

        wu@ubuntu:~/mse500-0.9.4$ ./buildall.sh mkimg

测试应用程序
------------

1. 把pkg用ftp烧到板子上:

2. 在Windows上telnet:

   进入Networking/Networking options/
   选中Network packet filtering framework 和Homeaccess Netfilter Test程序。
   
3. 编译pkg文件:

        wu@ubuntu:~/mse500-0.9.4$ ./buildall.sh mkimg

Crazy linking action
--------------------

I get 10 times more traffic from [Google] [1] than from
[Yahoo] [2] or [MSN] [3].

  [1]: http://google.com/        "Google"
  [2]: http://search.yahoo.com/  "Yahoo Search"
  [3]: http://search.msn.com/    "MSN Search"
