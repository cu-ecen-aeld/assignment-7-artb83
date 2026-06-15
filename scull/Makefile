# Comment/uncomment the following line to disable/enable debugging
#DEBUG = y


# Add your debugging flag (or not) to CFLAGS
ifeq ($(DEBUG),y)
  DEBFLAGS = -O -g -DSCULL_DEBUG # "-O" is needed to expand inlines
else
  DEBFLAGS = -O2
endif



EXTRA_CFLAGS += $(DEBFLAGS)

# 1. Fallback for Local Builds: If $(src) is empty, use standard make mechanics
ifeq ($(src),)
    # $(CURDIR) is a built-in GNU Make variable for your local path
    ccflags-y += -I$(CURDIR)/../include
else
    # 2. Fallback for Yocto/Kbuild: Use the standard kernel directory hooks
    ccflags-y += -I$(src)/../include
endif


ifneq ($(KERNELRELEASE),)
# call from kernel build system

scull-objs := main.o pipe.o access.o

obj-m	:= scull.o

else

KERNELDIR ?= /lib/modules/$(shell uname -r)/build
PWD       := $(shell pwd)

modules:
	$(MAKE) -C $(KERNELDIR) M=$(PWD) modules

endif



clean:
	rm -rf *.o *~ core .depend .*.cmd *.ko *.mod.c .tmp_versions *.mod modules.order *.symvers

depend .depend dep:
	$(CC) $(EXTRA_CFLAGS) -M *.c > .depend


ifeq (.depend,$(wildcard .depend))
include .depend
endif
