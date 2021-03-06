#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libmpdclient
PKG_VERSION:=2.14
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_HASH:=0a84e2791bfe3077cf22ee1784c805d5bb550803dffe56a39aa3690a38061372
PKG_SOURCE_URL:=https://www.musicpd.org/download/libmpdclient/2/

PKG_MAINTAINER:=Ted Hess <thess@kitschensync.net>
PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=COPYING

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libmpdclient
    TITLE:=libmpdclient
    SECTION:=libs
    CATEGORY:=Libraries
    URL:=http://mpd.wikia.com/wiki/ClientLib:libmpdclient
endef

define Package/libmpdclient/description
    A stable, documented, asynchronous API library for interfacing MPD in the C, C++ & Objective C languages.
endef

CONFIGURE_ARGS+= --disable-documentation

# Newer sources require meson/ninja to build so...
# Use our hacked-up version of the libmpdclient v2.11 autotools.
define Build/Prepare
	$(call Build/Prepare/Default)
	$(CP) ./autotools-files/* $(PKG_BUILD_DIR)/
endef

define Build/InstallDev
	$(INSTALL_DIR) \
		$(1)/opt/include \
		$(1)/opt/lib \
		$(1)/opt/lib/pkgconfig
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/lib/* \
		$(1)/opt/lib/
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/include/* \
		$(1)/opt/include/
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/lib/pkgconfig \
		$(1)/opt/lib/
endef

define Package/libmpdclient/install
	$(INSTALL_DIR) \
		$(1)/opt/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/lib/libmpdclient.so* \
		$(1)/opt/lib/
endef

$(eval $(call BuildPackage,libmpdclient))
