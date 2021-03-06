#
# Copyright (C) 2007-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=file
PKG_VERSION:=5.35
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://src.fedoraproject.org/lookaside/pkgs/file/ \
	http://download.openpkg.org/components/cache/file/ \
	ftp://ftp.astron.com/pub/file/
PKG_HASH:=30c45e817440779be7aac523a905b123cba2a6ed0bf4f5439e1e99ba940b5546

PKG_LICENSE:=BSD-2c
PKG_LICENSE_FILES:=COPYING

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/file/Default
  TITLE:=File type determination
  URL:=https://darwinsys.com/file/
  MAINTAINER:=Marko Ratkaj <marko.ratkaj@sartura.hr>
endef

define Package/file
$(call Package/file/Default)
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE+= utility
  DEPENDS:=+libmagic
endef

define Package/libmagic
$(call Package/file/Default)
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE+= library
  DEPENDS:=+zlib
endef

CONFIGURE_ARGS += \
	--enable-shared \
	--enable-static \
	--disable-libseccomp

MAKE_PATH := src

TARGET_CFLAGS += $(FPIC)

define Build/Compile/magic
	( cd $(PKG_BUILD_DIR)/magic/Magdir; \
		for f in `ls`; do \
			cat $$$${f}; \
		done \
	) > $(PKG_BUILD_DIR)/magic/magic
endef
Hooks/Compile/Post += Build/Compile/magic

define Build/Install/magic
	$(INSTALL_DIR) $(PKG_INSTALL_DIR)/opt/share/file
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/magic/magic $(PKG_INSTALL_DIR)/opt/share/file/
endef
Hooks/Install/Post += Build/Install/magic

define Build/InstallDev
	$(INSTALL_DIR) $(1)/opt/include
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/opt/include/magic.h $(1)/opt/include/
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/opt/lib/libmagic.{a,so*} $(1)/opt/lib/
endef

define Package/file/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/file $(1)/opt/bin/
	$(INSTALL_DIR) $(1)/opt/share/misc
	$(SED) "/^#/d" $(PKG_INSTALL_DIR)/opt/share/file/magic
	$(SED) "/^$$$$/d" $(PKG_INSTALL_DIR)/opt/share/file/magic
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/opt/share/file/magic $(1)/opt/share/misc/
endef

define Package/libmagic/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libmagic.so.* $(1)/opt/lib/
endef

$(eval $(call BuildPackage,file))
$(eval $(call BuildPackage,libmagic))
