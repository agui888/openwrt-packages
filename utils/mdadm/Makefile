include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=mdadm
PKG_VERSION:=3.2.3
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://fossies.org/linux/misc

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=A tool for managing Soft RAID under Linux
  URL:=http://www.kernel.org/pub/linux/utils/raid/mdadm/
  MAINTAINER:=Luka Perkov <openwrt@lukaperkov.net>
endef

define Package/$(PKG_NAME)/description
  Manages Mulitple Devices (sofware raid)
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/mdadm.init $(1)/etc/init.d/mdadm
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/mdadm $(1)/usr/sbin/
endef

$(eval $(call BuildPackage,mdadm))

