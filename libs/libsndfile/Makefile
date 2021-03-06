#
# Copyright (C) 2007-2018 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libsndfile
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/erikd/libsndfile.git
PKG_SOURCE_DATE:=2018-11-27
PKG_SOURCE_VERSION:=b4bd397ca74f4c72b9cabaae66fef0c3d5a8c527
PKG_MIRROR_HASH:=8e7cb3da240888c9c0e17d066e904d720c94dd379d3e5105df69797e2b321016

PKG_LICENSE:=LGPLv2.1
PKG_LICENSE_FILES:=COPYING

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/libsndfile
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=Library for reading/writing audio files
  URL:=http://www.mega-nerd.com/libsndfile/
  MAINTAINER:=Peter Wagner <tripolar@gmx.at>
endef

define Package/libsndfile/description
 libsndfile is a library of C routines for reading and writing files
 containing sampled audio data.
endef

CMAKE_OPTIONS += \
	-DBUILD_SHARED_LIBS:BOOL=ON \
	-DENABLE_EXTERNAL_LIBS:BOOL=FALSE \
	-DBUILD_REGTEST:BOOL=FALSE

TARGET_CFLAGS += $(FPIC)

define Build/InstallDev
	$(INSTALL_DIR) $(1)
	$(CP) $(PKG_INSTALL_DIR)/* $(1)/
endef

define Package/libsndfile/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libsndfile.so.* $(1)/opt/lib/
endef

$(eval $(call BuildPackage,libsndfile))
