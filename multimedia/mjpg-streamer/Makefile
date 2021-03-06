#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=mjpg-streamer
PKG_VERSION:=2018-04-14
PKG_RELEASE:=1
PKG_MAINTAINER:=Roger D <rogerdammit@gmail.com>, \
		Ted Hess <thess@kitschensync.net>

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/jacksonliam/mjpg-streamer.git
PKG_SOURCE_VERSION:=821c330ea6bbb5fbed98d48e00aac156e923161b
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_MIRROR_HASH:=f95e54bc95c808b9867bbca364e58b6c7e08cb26613205f8d87450ab9c899942

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

PKG_BUILD_DEPENDS:=MJPG_STREAMER_V4L2:libv4l

define Package/mjpg-streamer
  SECTION:=multimedia
  CATEGORY:=Multimedia
  TITLE:=MJPG-streamer
  DEPENDS:=+libpthread +libjpeg +MJPG_STREAMER_V4L2:libv4l @!LINUX_2_6_36
  URL:=https://github.com/jacksonliam/mjpg-streamer
  MENU:=1
endef

define Package/mjpg-streamer/description
 Streaming application for Linux-UVC compatible webcams
endef

define Package/mjpg-streamer/config
  source "$(SOURCE)/Config.in"
endef

define Package/mjpg-streamer/conffiles
/opt/etc/config/mjpg-streamer
endef

CAMBOZOLA:=cambozola-0.936.tar.gz

# Distribution URL doesn't always have the correct version
# Using the OpenWrt mirror provides a stable version
#      orig-URL:=http://www.andywilcock.com/code/cambozola
define Download/cambozola
  URL:=http://mirror2.openwrt.org/sources
  FILE:=$(CAMBOZOLA)
  MD5SUM:=35c45188aa9635aef2b745c35c311396
endef

# redefine prepare to extract to our build dir
# apply patches
define Build/Prepare
	rm -rf $(PKG_BUILD_DIR)/
	mkdir -p $(PKG_BUILD_DIR)/
	$(TAR) -xJf $(DL_DIR)/$(PKG_SOURCE) -C $(PKG_BUILD_DIR) --strip=2
	$(Build/Patch)
    # Fetch latest cambozola that works with latest Java(s)
    # Yes, I know this is ugly
    ifeq ($(CONFIG_MJPG_STREAMER_WWW),y)
	$(eval $(call Download,cambozola))
	$(TAR) -xf $(DL_DIR)/$(CAMBOZOLA) --strip=2 --wildcards \
		-C $(PKG_BUILD_DIR)/www */dist/cambozola.jar
    endif
endef

define Build/Configure
    $(RM) $(PKG_BUILD_DIR)/plugins/input_uvc/uvcvideo.h
endef

    TARGET_LDFLAGS+= -ljpeg

ifeq ($(CONFIG_MJPG_STREAMER_V4L2),y)
    TARGET_CFLAGS+= -DUSE_LIBV4L2
    TARGET_LDFLAGS+= -lv4l2
endif

define Package/mjpg-streamer/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/mjpg_streamer $(1)/opt/bin/
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S96mjpg-streamer $(1)/opt/etc/init.d/
	$(INSTALL_DIR) $(1)/opt/lib
ifeq ($(CONFIG_MJPG_STREAMER_INPUT_FILE),y)
	$(CP) $(PKG_BUILD_DIR)/input_file.so $(1)/opt/lib
endif
ifeq ($(CONFIG_MJPG_STREAMER_INPUT_UVC),y)
	$(CP) $(PKG_BUILD_DIR)/input_uvc.so $(1)/opt/lib
endif
ifeq ($(CONFIG_MJPG_STREAMER_INPUT_HTTP),y)
	$(CP) $(PKG_BUILD_DIR)/input_http.so $(1)/opt/lib
endif
ifeq ($(CONFIG_MJPG_STREAMER_OUTPUT_RTSP),y)
	$(CP) $(PKG_BUILD_DIR)/output_rtsp.so $(1)/opt/lib
endif
ifeq ($(CONFIG_MJPG_STREAMER_OUTPUT_UDP),y)
	$(CP) $(PKG_BUILD_DIR)/output_udp.so $(1)/opt/lib
endif
ifeq ($(CONFIG_MJPG_STREAMER_OUTPUT_FILE),y)
	$(CP) $(PKG_BUILD_DIR)/output_file.so $(1)/opt/lib
endif
ifeq ($(CONFIG_MJPG_STREAMER_OUTPUT_HTTP),y)
	$(CP) $(PKG_BUILD_DIR)/output_http.so $(1)/opt/lib
	$(INSTALL_DIR) $(1)/opt/share/www/webcam
    ifeq ($(CONFIG_MJPG_STREAMER_WWW),y)
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/www/* $(1)/opt/share/www/webcam
    else
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/www/stream_simple.html $(1)/opt/share/www/webcam/index.html
    endif
endif
endef

$(eval $(call BuildPackage,mjpg-streamer))
