#
# Copyright (C) 2006-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=logrotate
PKG_VERSION:=3.14.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:= https://github.com/logrotate/logrotate/releases/download/$(PKG_VERSION)
PKG_HASH:=4703bdc0e2df3b322f9dff0aafc99aa9172c9e4acae28b7c924cc7d4e5b29d55
PKG_LICENSE:=GPL-2.0

include $(INCLUDE_DIR)/package.mk

define Package/logrotate
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:=+libpopt
  TITLE:=rotates, compresses, and mails system logs
  URL:=https://github.com/logrotate/logrotate
  MAINTAINER:=Christian Beier <cb@shoutrlabs.com>
endef

define Package/logrotate/description
 logrotate is designed to ease administration of systems that generate large
 numbers of log files. It allows auto-matic rotation, compression, removal and
 mailing of log files. Each log file may be handled daily, weekly, monthly or
 when it grows too large.
endef

define Package/logrotate/conffiles
/opt/etc/logrotate.conf
endef

CONFIGURE_ARGS += --with-acl=no

EXTRA_CFLAGS += $(TARGET_CPPFLAGS) -Wno-nonnull-compare
EXTRA_LDFLAGS += $(TARGET_LDFLAGS)

define Build/Compile
	$(call Build/Compile/Default, \
		RPM_OPT_FLAGS:="$(TARGET_CFLAGS) $(EXTRA_CFLAGS)" \
		LDFLAGS="$(EXTRA_LDFLAGS)" \
		DEFAULT_MAIL_COMMAND="/opt/bin/mail" \
		STATEFILE="/opt/var/lib/logrotate.status" \
		logrotate \
	)
endef

define Package/logrotate/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/logrotate $(1)/opt/sbin/
	$(INSTALL_DIR) $(1)/opt/etc/logrotate.d
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/examples/logrotate-default $(1)/opt/etc/logrotate.conf
endef

$(eval $(call BuildPackage,logrotate))
