#
# Copyright (C) 2006-2008 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=cyrus-sasl
PKG_VERSION:=2.1.25
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://ftp.andrew.cmu.edu/pub/cyrus-mail/
PKG_MD5SUM:=

PKG_LICENSE:=BSD-4c BSD
PKG_LICENSE_FILES:=COPYING cmulocal/COPYING saslauthd/COPYING

PKG_BUILD_DEPENDS:=libopenssl

PKG_FIXUP:=autoreconf
PKG_MACRO_PATHS:=cmulocal config ../cmulocal ../config
PKG_AUTOMAKE_PATHS:=. saslauthd sasldb
PKG_REMOVE_FILES:=aclocal.m4 saslauthd/aclocal.m4 config/libtool.m4

include $(INCLUDE_DIR)/package.mk

define Package/libsasl2
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=A general purpose authentication library
  URL:=http://asg.web.cmu.edu/sasl/
  DEPENDS:=+krb5-libs +libopenssl
endef

define Package/sasl2
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=A general purpose authenticator
  URL:=http://asg.web.cmu.edu/sasl/
  DEPENDS:=+libsasl2
endef

TARGET_CFLAGS += $(FPIC) -I$(STAGING_DIR)/usr/include/krb5
CONFIGURE_ARGS += \
	--enable-shared \
	--enable-static \
	--disable-sample \
	--disable-java \
	--disable-alwaystrue \
	--disable-checkapop \
	--enable-cram \
	--enable-digest \
	--without-auth-sasldb \
	--disable-otp \
	--disable-srp \
	--disable-srp-setpass \
	--disable-krb4 \
	--enable-gssapi \
	--enable-gss_mutexes \
	--with-gss_impl=mit \
	--enable-plain \
	--enable-anon \
	--disable-login \
	--disable-ntlm \
	--disable-sql \
	--disable-ldapdb \
	--without-dblib \
	--without-gdbm \
	--with-devrandom="/dev/urandom" \
	--without-pam \
	--with-saslauthd=/var/run/saslauthd \
	--without-authdaemond \
	--without-pwcheck \
	--with-ipctype=unix \
	--with-openssl="$(STAGING_DIR)/usr" \
	--without-des \
	--without-opie \
	--without-ldap \
	--without-mysql \
	--without-pgsql \
	--without-sqlite \
	--without-rc4 \
	--without-dmalloc \
	--without-sfio \
	--disable-sample

define Build/Prepare
	$(call Build/Prepare/Default)
	$(SED) 's!-lcom_err!-l:libcom_err.so.3!g' $(PKG_BUILD_DIR)/saslauthd/aclocal.m4
	$(SED) 's!-lcom_err!-l:libcom_err.so.3!g' $(PKG_BUILD_DIR)/cmulocal/com_err_link.m4
	$(SED) 's!-lcom_err!-l:libcom_err.so.3!g' $(PKG_BUILD_DIR)/cmulocal/com_err.m4
	$(SED) 's!-lcom_err!-l:libcom_err.so.3!g' $(PKG_BUILD_DIR)/cmulocal/sasl2.m4
	$(SED) 's!-lcom_err!-l:libcom_err.so.3!g' $(PKG_BUILD_DIR)/aclocal.m4
	$(SED) 's!-lcom_err!-l:libcom_err.so.3!g' $(PKG_BUILD_DIR)/config/kerberos_v4.m4
	$(SED) 's!-lcom_err!-l:libcom_err.so.3!g' $(PKG_BUILD_DIR)/cmulocal/afs.m4
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)/include \
		CC="$(HOSTCC)" \
		LINK="$(HOSTCC) -o makemd5 -lc" \
		CFLAGS="" \
		CPPFLAGS="" \
		makemd5
	$(MAKE) -C $(PKG_BUILD_DIR) \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		all install
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/include/sasl $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libsasl2.{a,so*} $(1)/usr/lib/
	ln -sf libsasl2.a $(1)/usr/lib/libsasl.a
	ln -sf libsasl2.so $(1)/usr/lib/libsasl.so
	$(INSTALL_DIR) $(1)/usr/lib/sasl2
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/sasl2/lib*.{a,so*} $(1)/usr/lib/sasl2/
endef

define Package/libsasl2/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libsasl2.so* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/sasl2
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/sasl2/lib*.so* $(1)/usr/lib/sasl2/
endef

define Package/sasl2/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/saslauthd $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/saslauthd.init $(1)/etc/init.d/saslauthd
endef

$(eval $(call BuildPackage,libsasl2))
$(eval $(call BuildPackage,sasl2))
