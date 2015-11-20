MME communication between eoc and ar9331 wifi router
===================================================

To make a project which accomplish the communication between eoc master and wifi router.eoc master can get&set wifi router's parameter successfully.


Weekly summary
===================================================


Week1
-------------------------------

understand the mme source code on eoc. 


Week2
-------------------------------

[Debug ar9331&mse510 mac communiaction](http://slides.com/wufengyi/deck#/)


Week3
-------------------------------

[Add vlan to mac packet](http://slides.com/wufengyi/add#/)

[Create a daemon on linux](http://slides.com/wufengyi/deck-3#/)

Week4
-------------------------------

In this week,I make a lot progress debuging communication betwween eoc and 9331 wifi.Solid test in requesting and responsing. 

so many useful debug techniques help me a lot.

```c
/*
 *      netif_receive_skb - process receive buffer from network
 *      @skb: buffer to process
 *
 *      netif_receive_skb() is the main receive data processing function.
 *      It always succeeds. The buffer may be dropped during processing
 *      for congestion control or by the protocol layers.
 *
 *      This function may only be called from softirq context and interrupts
 *      should be enabled.
 *
 *      Return values (usually ignored):
 *      NET_RX_SUCCESS: no congestion
 *      NET_RX_DROP: packet was dropped
 */
int netif_receive_skb(struct sk_buff *skb)
{
        struct packet_type *ptype, *pt_prev;
        struct net_device *orig_dev;
        int ret = NET_RX_DROP;
        __be16 type;
        struct ethhdr *mh = eth_hdr(skb);
#if 0
        printk("[Debug]Coming packet protocol is %d\n",skb->protocol);
        printk(KERN_EMERG "Source MAC=%x:%x:%x:%x:%x:%x\n",mh->h_source[0],mh->h_source[1],mh->h_source[2],mh->h_source[3],mh->h_source[4],mh->h_source[5]);
        printk(KERN_EMERG "Destination MAC=%x:%x:%x:%x:%x:%x\n",mh->h_dest[0],mh->h_dest[1],mh->h_dest[2],mh->h_dest[3],mh->h_dest[4],mh->h_dest[5]);
#endif
..............
}
```

This function is one of the most important function in network.When packet go out of the driver layer,then It will jump in this function.So,you can debug your network coming packet inside netif_receive_skb function.I think every one who is working on network development should read the 《linux network internals》book.very very useful.


Other thing I learn this week:

1:[Creating a shared and static library with the gnu compiler](http://www.adp-gmbh.ch/cpp/gcc/create_lib.html)

2:[linking symbol](http://www.yolinux.com/TUTORIALS/LibraryArchives-StaticAndDynamic.html)

3:read book 《Embedded Linux Primer》 chapter5,6,11,about linux system initialization,userspace initialization and busybox.

4:code to genarate a shared library:

```c
STAT_LIB=libhamme.a
DYN_LIB=libhamme.so
OBJPATH=obj
SRCPATH=src
INCPATH=inc
LIBPATH = .

CLEO_DIR = ../..
LINUX_DIR = $(CLEO_DIR)/linux-2.6.25.10-spc300

ifeq ($(CC_FOR_TARGET),) #direct compile
CC=arm-linux-gcc
AR=arm-linux-ar
CC_WITH_CFLAGS=$(CC) -g -Os
CC_WITHOUT_CFLAGS=$(CC)
else #compile from buildroot
CC_WITH_CFLAGS=$(CC)
CC_WITHOUT_CFLAGS=$(CC_FOR_TARGET)
endif

RESPONSE_FILE = extra_flags
INCLUDES = -I$(INCPATH) \
                   -I$(CLEO_DIR)/include \
                   -I$(LINUX_DIR)/include

EXTRA_CFLAGS = $(INCLUDES) -MMD -Wall \
                           @$(CLEO_DIR)/$(RESPONSE_FILE) \

SRCS=$(subst $(SRCPATH)/,,$(wildcard $(SRCPATH)/*.c))
DYN_OBJS=$(addprefix $(OBJPATH)/,$(SRCS:.c=.dyn.o))
STAT_OBJS=$(addprefix $(OBJPATH)/,$(SRCS:.c=.stat.o))
DYN_DEPS=$(patsubst %o,%d,$(DYN_OBJS))
STAT_DEPS=$(patsubst %o,%d,$(STAT_OBJS))

all: $(STAT_LIB) $(DYN_LIB)

$(STAT_LIB): $(STAT_OBJS)
        $(AR) cr $(LIBPATH)/$@ $(STAT_OBJS)

$(DYN_LIB): $(DYN_OBJS)
        $(CC_WITHOUT_CFLAGS) -shared -fPIC -o $@ $(DYN_OBJS)

$(OBJPATH)/%.stat.o: $(SRCPATH)/%.c
        $(CC_WITH_CFLAGS) $(EXTRA_CFLAGS) -o $@ -c $<

$(OBJPATH)/%.dyn.o: $(SRCPATH)/%.c
        $(CC_WITH_CFLAGS) $(EXTRA_CFLAGS) -fPIC -o $@ -c $<

$(DYN_OBJS) $(STAT_OBJS): | $(OBJPATH)

$(OBJPATH):
        mkdir $(OBJPATH)
```

5:Code to use shared library：

```c
BIN=ha-mmed
OBJPATH=obj
SRCPATH=src
INCPATH=.

CLEO_DIR = ../..
LINUX_DIR = $(CLEO_DIR)/linux-2.6.25.10-spc300
HA_LIBMME_DIR = $(CLEO_DIR)/application/libhamme

HA_LIBMME_SO_BIN = $(HA_LIBMME_DIR)/libhamme.so

CC=arm-linux-gcc
CC_WITH_CFLAGS=$(CC) -I/opt/spidcom/spc300/usr/include -g -Os
CC_WITHOUT_CFLAGS=$(CC)

RESPONSE_FILE = extra_flags
INCLUDES = -I$(INCPATH) \
                -I$(HA_LIBMME_DIR)/inc \
                   -I$(LINUX_DIR)/include

EXTRA_CFLAGS = $(INCLUDES) -MMD -Wall \
                           @$(CLEO_DIR)/$(RESPONSE_FILE)

SRCS=$(subst $(SRCPATH)/,,$(wildcard $(SRCPATH)/*.c))
OBJS=$(addprefix $(OBJPATH)/,$(SRCS:.c=.o))
DEPS=$(patsubst %o,%d,$(OBJS))

all: $(BIN)

$(BIN): $(OBJS) $(HA_LIBMME_SO_BIN)
        $(CC_WITHOUT_CFLAGS) -L$(HA_LIBMME_DIR) -lhamme -o $@ $(OBJS)

$(OBJPATH)/%.o: $(SRCPATH)/%.c
        $(CC_WITH_CFLAGS) $(EXTRA_CFLAGS) -o $@ -c $<

$(OBJS): | $(OBJPATH)

$(OBJPATH):
        mkdir $(OBJPATH)

$(HA_LIBMME_SO_BIN):
         $(error libhamme output files are not found)

-include $(DEPS)

.PHONY: all clean distclean

distclean: clean

clean:
        rm -rf $(BIN) $(OBJPATH)

```

week5：

A lot of test this week.

week6：
[reading this](https://www.youtube.com/playlist?list=PLGeM09tlguZTP9-9nMQNGiT_2PPFay0Cs)


