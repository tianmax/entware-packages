#
# Copyright (C) 2010-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=grep
PKG_VERSION:=3.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=@GNU/grep
PKG_HASH:=db625c7ab3bb3ee757b3926a5cfa8d9e1c3991ad24707a83dde8a5ef2bf7a07e

PKG_LICENSE:=GPL-3.0
PKG_LICENSE_FILES:=COPYING
PKG_MAINTAINER:=Julen Landa Alustiza <julen@zokormazo.info>

PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/grep
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=grep search utility - full version
  DEPENDS:=+libpcre
  URL:=https://www.gnu.org/software/grep/
endef

define Package/grep/description
The grep command searches one or more input files for lines
containing a match to a specified pattern. By default, grep
prints the matching lines.
endef

define Package/grep/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/grep $(1)/opt/bin/grep
endef

$(eval $(call BuildPackage,grep))
