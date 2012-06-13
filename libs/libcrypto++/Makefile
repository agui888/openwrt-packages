#
# Copyright (C) 2009-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libcrypto++
PKG_VERSION:=5.6.1
PKG_RELEASE:=2


PKG_SOURCE:=cryptopp561.zip
PKG_SOURCE_URL:=http://www.cryptopp.com

#PKG_SOURCE:=$(PKG_NAME)_$(PKG_VERSION).debian.tar.gz
#PKG_SOURCE:=$(PKG_NAME)_$(PKG_VERSION).orig.tar.gz
#PKG_SOURCE_URL:=http://ftp.de.debian.org/debian/pool/main/libc/libcrypto++/
#PKG_MD5SUM:=681f830145bf93cbc16113eaf61fbe2a

PKG_FIXUP:=autoreconf
PKG_REMOVE_FILES:=aclocal.m4 GNUmakefile
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libcryptoxx
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libpthread +libstdcpp
  TITLE:=Crypto++ is library for creating C++ programs which use cryptographic algorithms
  URL:=http://packages.debian.org/sid/libcrypto++-dev
endef

define Package/libcryptoxx/description
  Crypto++ is library for creating C++ programs which use cryptographic algorithms.
  The library uses a Pipes & Filters architecture with heavy use of templates and
  abstract base classes.
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	unzip -d $(PKG_BUILD_DIR) $(DL_DIR)/$(PKG_SOURCE)
	$(call Build/Patch/Default)
endef

CONFIGURE_ARGS += \
	--enable-static \
	--enable-shared

TARGET_LDFLAGS += \
	-lpthread

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/crypto++
	$(CP) $(PKG_INSTALL_DIR)/usr/include/crypto--/* $(1)/usr/include/crypto++/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libcrypto++.{a,so*} $(1)/usr/lib/
endef

define Package/libcryptoxx/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libcrypto++.so* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libcryptoxx))
