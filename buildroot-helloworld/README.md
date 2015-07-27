Buildroot tiny test
================================

为了熟悉buildroot相关的一些原理，方便以后在工作中熟练添加package，对Buildroot进行了一个简单的测试。

Buildroot简介
-------------------------

*[Making Embedded Linux Easy ](http://www.buildroot.org/).*

MSE500添加ha-helloworld package的步骤
-------------------------------

####1. 在下面的路径新建ha-helloworld文件夹:

        wufengyi@Server-U12:~/mse500-0.9.4/buildroot/package$ pwd
        /home/wufengyi/mse500-0.9.4/buildroot/package
        wufengyi@Server-U12:~/mse500-0.9.4/buildroot/package$ mkdir ha-helloworld
        wufengyi@Server-U12:~/mse500-0.9.4/buildroot/package$ cd ha-helloworld/

####2. 创建Config.in文件及Makefile:

        wufengyi@Server-U12:~/mse500-0.9.4/buildroot/package/ha-helloworld$ pwd
        /home/wufengyi/mse500-0.9.4/buildroot/package/ha-helloworld
        wufengyi@Server-U12:~/mse500-0.9.4/buildroot/package/ha-helloworld$ vim Config.in 
        wufengyi@Server-U12:~/mse500-0.9.4/buildroot/package/ha-helloworld$ vim ha-helloworld.mk 

####3. 分别添加下面的内容到各个文件:

Config.in

```config
  1 config BR2_PACKAGE_HELLOWORLD
  2         bool "helloworld"
  3         help
  4           A sample application to understand how buildroot works
  5
```
   
ha-helloworld.mk
```makefile
{
  1 #############################################################
  2 #
  3 # helloworld
  4 #
  5 #############################################################
  6 #
  7 HA_HELLOWORLD_VERSION:=0.0.1
  8 HA_HELLOWORLD_SITE:=$(BASE_DIR)/../application/ha-helloworld
  9 HA_HELLOWORLD_NAME:=ha-helloworld-$(HA_HELLOWORLD_VERSION)
 10 HA_HELLOWORLD_BUILD_DIR=$(BUILD_DIR)/ha-helloworld-$(HA_HELLOWORLD_VERSION)
 11
 12 $(HA_HELLOWORLD_BUILD_DIR)/.unpacked:
 13         ln -s $(HA_HELLOWORLD_SITE) $(HA_HELLOWORLD_BUILD_DIR)
 14         touch $(HA_HELLOWORLD_BUILD_DIR)/.unpacked
 15
 16 $(HA_HELLOWORLD_BUILD_DIR)/.configured: $(HA_HELLOWORLD_BUILD_DIR)/.unpacked
 17         touch $(HA_HELLOWORLD_BUILD_DIR)/.configured
 18
 19 $(HA_HELLOWORLD_BUILD_DIR)/helloworld: $(HA_HELLOWORLD_BUILD_DIR)/.configured
 20         (cd $(HA_HELLOWORLD_BUILD_DIR);$(TARGET_CONFIGURE_OPTS) $(MAKE))
 21
 22 $(TARGET_DIR)/usr/bin/helloworld: $(HA_HELLOWORLD_BUILD_DIR)/helloworld
 23         cp -af $(HA_HELLOWORLD_BUILD_DIR)/helloworld $(TARGET_DIR)/usr/bin/
 24
 25 ha-helloworld: $(TARGET_DIR)/usr/bin/helloworld
 26
 27 ha-helloworld-source:
 28
 29 ha-helloworld-clean:
 30         rm -f $(HA_HELLOWORLD_BUILD_DIR)/*.o $(HA_HELLOWORLD_BUILD_DIR)/helloworld
 31
 32 ha-helloworld-dirclean:
 33         rm -rf $(HA_HELLOWORLD_BUILD_DIR)
 34
 35 #############################################################
 36 #
 37 # Toplevel Makefile options
 38 #
 39 #############################################################
 40 ifeq ($(strip $(BR2_PACKAGE_HELLOWORLD)),y)
 41 TARGETS+=ha-helloworld
 42 endif
}
```
####4. 打开buildroot/package下面的Config.in:

        wufengyi@Server-U12:~/mse500-0.9.4/buildroot/package$ pwd
        /home/wufengyi/mse500-0.9.4/buildroot/package
        wufengyi@Server-U12:~/mse500-0.9.4/buildroot/package$ vim Config.in
        
####5. 添加新的source到Config.in

```c
{
308 source "package/ha-helloworld/Config.in"
}
```

####6. 在application文件夹下添加新文件夹

        wufengyi@Server-U12:~/mse500-0.9.4/application$ pwd
        /home/wufengyi/mse500-0.9.4/application
        wufengyi@Server-U12:~/mse500-0.9.4/application$ mkdir ha-helloworld
        wufengyi@Server-U12:~/mse500-0.9.4/application$ cd ha-helloworld/

####7. 创建Makefile和helloworld.c 

        wufengyi@Server-U12:~/mse500-0.9.4/application/ha-helloworld$ pwd
        /home/wufengyi/mse500-0.9.4/application/ha-helloworld
        wufengyi@Server-U12:~/mse500-0.9.4/application/ha-helloworld$ vim Makefile
        wufengyi@Server-U12:~/mse500-0.9.4/application/ha-helloworld$ mkdir src
        wufengyi@Server-U12:~/mse500-0.9.4/application/ha-helloworld$ cd src
        wufengyi@Server-U12:~/mse500-0.9.4/application/ha-helloworld/src$ vim helloworld.c

####8. 分别添加下面的内容到各个文件:

Makefile

```makefile
BIN=helloworld
OBJPATH=obj
SRCPATH=src
INCPATH=.

CLEO_DIR = ../..
LINUX_DIR = $(CLEO_DIR)/linux-2.6.25.10-spc300

ifeq ($(CC_FOR_TARGET),) #direct compile
CC=arm-linux-gcc
CC_WITH_CFLAGS=$(CC) -I/opt/spidcom/spc300/usr/include -g -Os
CC_WITHOUT_CFLAGS=$(CC)
else #compile from buildroot
CC_WITH_CFLAGS=$(CC)
CC_WITHOUT_CFLAGS=$(CC_FOR_TARGET)
endif

RESPONSE_FILE = extra_flags
INCLUDES = -I$(INCPATH) \
                   -I$(LINUX_DIR)/include

EXTRA_CFLAGS = $(INCLUDES) -MMD -Wall \
                           @$(CLEO_DIR)/$(RESPONSE_FILE)

SRCS=$(subst $(SRCPATH)/,,$(wildcard $(SRCPATH)/*.c))
OBJS=$(addprefix $(OBJPATH)/,$(SRCS:.c=.o))
DEPS=$(patsubst %o,%d,$(OBJS))

all: $(BIN)

$(BIN): $(OBJS)
        $(CC_WITHOUT_CFLAGS) -o $@ $(OBJS)

$(OBJPATH)/%.o: $(SRCPATH)/%.c
        $(CC_WITH_CFLAGS)  -o $@ -c $<

$(OBJS): | $(OBJPATH)

$(OBJPATH):
        mkdir $(OBJPATH)

-include $(DEPS)

.PHONY: all clean

clean:
        rm -f $(OBJS) $(DEPS) $(BIN)
        rmdir $(OBJPATH)

```

helloworld.c

```c
{
#include <stdio.h>

int main()
{

        printf("Hello world from HomeAcess BuildRoot!!!\n");

        return 0;
}
}
```
配置编译应用程序
------------

1. 进入buildroot下，配置kernel:

        wu@ubuntu:~/mse500-0.9.4$ cd buildroot
        wu@ubuntu:~/mse500-0.9.4/buildroot$ make menuconfig
        
2. 勾选helloworld程序:

   进入Package Selection for the target/Networking，
   选中helloworld程序。
   
3. 编译pkg文件:

        wu@ubuntu:~/mse500-0.9.4$ ./buildall.sh mkimg

测试应用程序
------------

1. 把pkg用ftp烧到目标机上:

2. 串口连接到目标机:

3. 执行文件:
 
        # cd /usr/bin
        # ./helloworld
        Hello world from HomeAcess BuildRoot!!!

有用的资源
--------------------

*[Buildroot adding package](http://buildroot.uclibc.org/downloads/manual/manual.html#adding-packages).*


