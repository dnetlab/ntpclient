# ntpclient
ntpclient is an NTP (RFC-1305) client for unix-alike computers. Its functionality is a small subset of xntpd, but IMHO performs better (or at least has the potential to function better) within that limited scope. Since it is much smaller than xntpd, it is also more relevant for embedded computers.  ntpclient is Copyright (C) 1997-2015 Larry Doolittle, and may be freely copied and modified according to the terms of the GNU General Public License, version 2.

## Makefile For Openwrt
```
#
# Copyright (C) 2006-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=net-ntpclient
PKG_VERSION:=2010_365
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/dnetlab/ntpclient.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=afff0f0399e209ec4e9436da96e800979e679f4a
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz

PKG_MAINTAINER:=Ted Hess <thess@kitschensync.net>

PKG_LICENSE:=GPL-2.0

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
  SECTION:=net
  CATEGORY:=Dnetlab
  TITLE:=NTP (Network Time Protocol) client
  URL:=http://doolittle.icarus.com/ntpclient/
  DEPENDS:=+librt
endef

define Package/$(PKG_NAME)/description
	NTP client for setting system time from NTP servers.
endef

define Package/$(PKG_NAME)/conffiles
/etc/config/ntpclient
endef

MAKE_FLAGS += \
	all adjtimex

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/etc/hotplug.d/iface
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/files/ntpclient.hotplug $(1)/etc/hotplug.d/iface/20-ntpclient
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/files/ntpclient.init $(1)/etc/init.d/ntpc
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ntpclient $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/adjtimex $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/rate.awk $(1)/usr/sbin/
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
```