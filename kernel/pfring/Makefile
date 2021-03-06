#
# Copyright (C) 2017 Banglang Huang
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=pf-ring
PKG_VERSION:=7.2.0
PKG_RELEASE:=1
PKG_MAINTAINER:=Banglang Huang <banglang.huang@foxmail.com>

PKG_LICENSE:=GPL-2.0

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/ntop/PF_RING/tar.gz/$(PKG_VERSION)?
PKG_HASH:=5d349ac37a6ece5966bf606a6f131d628b98d88654c2f502d3c4b8bbf6ef9796

PKG_BUILD_DIR:=$(KERNEL_BUILD_DIR)/PF_RING-$(PKG_VERSION)

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

CONFIGURE_PATH:=kernel
MAKE_PATH:=kernel

define KernelPackage/pf-ring
  SUBMENU:=Network Support
  TITLE:=PF_RING Kernel driver
  FILES:=$(PKG_BUILD_DIR)/kernel/pf_ring.ko
  AUTOLOAD:=$(call AutoLoad,90,pf_ring,1)
endef

define KernelPackage/pf-ring/description
  Kernel module for libpf-ring package
endef

EXTRA_CFLAGS += \
	-I$(PKG_BUILD_DIR)/kernel

MAKE_OPTS := \
	ARCH="$(LINUX_KARCH)" \
	CROSS_COMPILE="$(TARGET_CROSS)" \
	SUBDIRS="$(PKG_BUILD_DIR)/kernel" \
	CFLAGS="$(TARGET_CFLAGS)" \
	EXTRA_CFLAGS="$(EXTRA_CFLAGS)"

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/linux
	$(CP) $(PKG_INSTALL_DIR)/usr/include/linux/*.h $(1)/usr/include/linux
endef

define Build/Compile
	+$(MAKE) -C "$(LINUX_DIR)" \
		$(MAKE_OPTS) \
		modules
endef

#$(eval $(call KernelPackage,pf-ring))
