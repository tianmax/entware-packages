#
# Copyright (C) 2007-2017 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=django
PKG_VERSION:=1.11.16
PKG_RELEASE=1
PKG_LICENSE:=BSD-3-Clause

PKG_SOURCE:=Django-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://files.pythonhosted.org/packages/source/D/Django
PKG_HASH:=29268cc47816a44f27308e60f71da635f549c47d8a1d003b28de55141df75791
PKG_BUILD_DIR=$(BUILD_DIR)/Django-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include ../python-package.mk

define Package/django
    SUBMENU:=Python
    SECTION:=lang
    CATEGORY:=Languages
    TITLE:=The web framework for perfectionists with deadlines.
    MAINTAINER:=Gergely Kiss <mail.gery@gmail.com>
    URL:=https://www.djangoproject.com/
    DEPENDS:=+python
endef

define Package/django/description
    The web framework for perfectionists with deadlines.
endef

define Build/Compile
	$(call Build/Compile/PyMod,,install --prefix=/opt --root=$(PKG_INSTALL_DIR))
endef

define Build/InstallDev
	### fix exec path
	$(SED) 's|/opt/bin|$(STAGING_DIR_HOSTPKG)/bin|' $(PKG_INSTALL_DIR)/opt/bin/django-admin.py \
		$(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/django/bin/django-admin.py
	$(INSTALL_DIR) $(1)$(PYTHON_PKG_DIR)
	$(INSTALL_DIR) $(1)/opt/bin
	$(CP) \
	    $(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/* \
	    $(1)$(PYTHON_PKG_DIR)
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/django-admin.py \
	    $(1)/opt/bin
endef

define Package/django/install
	$(INSTALL_DIR) $(1)$(PYTHON_PKG_DIR)
	$(INSTALL_DIR) $(1)/opt/bin
	$(CP) \
	    $(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/* \
	    $(1)$(PYTHON_PKG_DIR)
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/django-admin.py \
	    $(1)/opt/bin
	# fix python exec path in scripts
	find $(PKG_BUILD_DIR) -name *.py | xargs $(SED) 's|/usr/bin/env python|/opt/bin/python|'
	sed -i 's/#!.*/#!\/opt\/bin\/python/g' $(1)/opt/bin/django-admin.py
endef

$(eval $(call BuildPackage,django))
