#
# Copyright (C) 2010-2012, 2014 ARM Limited. All rights reserved.
# 
# This program is free software and is provided to you under the terms of the GNU General Public License version 2
# as published by the Free Software Foundation, and any use by you of this program is subject to the terms of such GNU licence.
# 
# A copy of the licence is included with the program, and can also be obtained from Free Software
# Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301, USA.
#

# For each arch check: CROSS_COMPILE , KDIR , CFLAGS += -DARCH

ARCH := arm
BUILD := debug

WM_CC := /home/sachin/github/apc-rock-toolchain/mybin/arm_1103_le-
WM_KERNEL := /home/sachin/kernel/apc-rock-II-kernel/

check_cc2 = \
	$(shell if $(1) -S -o /dev/null -xc /dev/null > /dev/null 2>&1; \
	then \
		echo "$(2)"; \
	else \
		echo "$(3)"; \
	fi ;)

## Check that required parameters are supplied.
#ifeq ($(CONFIG),)
#CONFIG := default
#endif
#ifeq ($(CPU)$(KDIR),)
#$(error "KDIR or CPU must be specified.")
#endif

# Get any user defined KDIR-<names> or maybe even a hardcoded KDIR
-include KDIR_CONFIGURATION

# Define host system directory
KDIR-$(shell uname -m):=/lib/modules/$(shell uname -r)/build

ifeq ($(ARCH), arm)
# when compiling for ARM we're cross compiling
export CROSS_COMPILE ?= $(call check_cc2, arm-linux-gnueabi-gcc, arm-linux-gnueabi-, arm-none-linux-gnueabi-)
endif

# look up KDIR based om CPU selection
KDIR := $(KDIR-$(CPU))

export CONFIG

export CONFIG_UMP := m
ifeq ($(BUILD),debug)
export CONFIG_UMP_DEBUG := y
else
export CONFIG_UMP_DEBUG := n
endif

#ifeq ($(KDIR),)
#$(error No KDIR found for platform $(CPU))
#endif

ARCH := arm
CC := ${WM_CC}
KVER  := 3.4.5
KSRC := ${WM_KERNEL}
MODDESTDIR := $(PWD)

all: modules

modules:
	$(MAKE) ARCH=$(ARCH) CROSS_COMPILE=${CC} -C $(KSRC) M=$(shell pwd)  modules

kernelrelease:
	$(MAKE) -C $(KSRC) kernelrelease

clean:
	$(MAKE) -C $(KSRC) M=$(CURDIR) clean
	$(MAKE) -C $(KSRC) M=$(CURDIR)/../mali clean
