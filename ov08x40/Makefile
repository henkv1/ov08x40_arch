SHELL := /bin/sh
KVER ?= $(if $(KERNELRELEASE),$(KERNELRELEASE),$(shell uname -r))
KSRC ?= $(if $(KERNEL_SRC),$(KERNEL_SRC),/lib/modules/$(KVER)/build)
MODLIST := ov08x40

ifneq ("$(INSTALL_MOD_PATH)", "")
DEPMOD_ARGS = -b $(INSTALL_MOD_PATH)
else
DEPMOD_ARGS =
endif

ifneq ("","$(wildcard $(MODDESTDIR)/*.ko.gz)")
COMPRESS_GZIP := y
endif
ifneq ("","$(wildcard $(MODDESTDIR)/*.ko.xz)")
COMPRESS_XZ := y
endif
ifneq ("","$(wildcard $(MODDESTDIR)/*.ko.zst)")
COMPRESS_ZSTD := y
endif

export CONFIG_ICAMERA_OV08X40 = m
EXTRA_CFLAGS += -O2 -std=gnu11 -Wno-declaration-after-statement

obj-$(CONFIG_ICAMERA_OV08X40) += ov08x40.o

ccflags-y += -D__CHECK_ENDIAN__

all: 
	$(MAKE) -j`nproc` -C $(KSRC) M=$$PWD modules
	
install: all
	@install -D -m 644 -t $(MODDESTDIR) *.ko

ifeq ($(COMPRESS_GZIP), y)
	@gzip -f $(MODDESTDIR)/*.ko
endif
ifeq ($(COMPRESS_XZ), y)
	@xz -f -C crc32 $(MODDESTDIR)/*.ko
endif
ifeq ($(COMPRESS_ZSTD), y)
	@zstd -f -q --rm $(MODDESTDIR)/*.ko
endif

	@depmod $(DEPMOD_ARGS) -a $(KVER)

uninstall:
	@for mod in $(MODLIST); do \
		rmmod -s $$mod || true; \
	done
	@rm -vf $(MODDESTDIR)/ov08x40.ko*
	@depmod $(DEPMOD_ARGS)

clean:
	$(MAKE) -C $(KSRC) M=$$PWD clean


