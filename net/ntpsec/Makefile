#
# Copyright (C) 2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=NTPsec
#PKG_VERSION:=0_9_1
PKG_VERSION:=0.9.4-20160708
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)_$(PKG_VERSION).tar.gz
#PKG_SOURCE_URL:=ftp://ftp.ntpsec.org/pub/releases/
#PKG_MD5SUM:=d1005cae1c8ff3e9316fae7fe518e2f5

PKG_REV:=47fb7e7bd17f5db536ccce8b3a7e6490cfd757ed
PKG_SOURCE_URL:=https://gitlab.com/NTPsec/ntpsec.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_BUILD_DEPENDS:=pps-tools
#PKG_BUILD_DIR:=$(BUILD_DIR)/ntpsec-$(PKG_NAME)_$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/ntpsecd/Default
  SUBMENU:=Time Synchronization
  SECTION:=net
  CATEGORY:=Network
  TITLE:=NTPsec
  URL:=https://www.ntpsec.org/
  DEPENDS:=+kmod-pps +libopenssl
endef

define Package/ntpsecd/Default/description
 A secure, hardened, and improved implementation of Network Time Protocol
 derived from NTP Classic, Dave Mills's original.
endef

define Package/ntpsecd
$(call Package/ntpsecd/Default)
  TITLE+= server
  USERID:=ntp=123:ntp=123
  DEPENDS+= +libcap
endef

define Package/ntpsecd/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains the ntpd server.
endef

define Package/ntpsecdate
$(call Package/ntpsecd/Default)
  TITLE+=date
  DEPENDS+= +ntpsecdig
endef

define Package/ntpsecdate/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains ntpdate.
endef

define Package/ntpsecdig
$(call Package/ntpsecd/Default)
  TITLE+=dig
  DEPENDS+= +libevent2-core +libevent2-pthreads
endef

define Package/ntpsecdig/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains ntpdig.
endef

define Package/ntpseckeygen
$(call Package/ntpsecd/Default)
  TITLE+=keygen
endef

define Package/ntpseckeygen/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains the ntpkeygen.
endef

define Package/ntpsec-utils
$(call Package/ntpsecd/Default)
  TITLE+= utilities
endef

define Package/ntpsec-utils/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains ntpfrob and ntpq.
endef

define Package/ntpsecd/conffiles
/etc/ntp.conf
endef

define Build/Compile
	cd $(PKG_BUILD_DIR) && \
	$(HOST_CONFIGURE_VARS) \
	./waf configure && \
	./waf build_host && \
	./waf install_host && \
	cd $(PKG_BUILD_DIR) && \
	$(CONFIGURE_VARS) \
	./waf configure \
		--cross-compiler="$(TARGET_CC)" \
		--disable-debug \
		--disable-manpage \
		--disable-mdns-registration \
		--enable-crypto \
		--prefix=/usr \
		--refclock="shm" && \
	./waf build_main && \
	DESTDIR="$(PKG_INSTALL_DIR)" ./waf install_main
endef

define Package/ntpsecd/install
	$(INSTALL_DIR) $(1)/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/ntpd $(1)/sbin/
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_CONF) ./files/ntp.conf $(1)/etc/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/ntpd.init $(1)/etc/init.d/ntpd
	$(INSTALL_DIR) $(1)/etc/hotplug.d/iface
	$(INSTALL_BIN) ./files/ntpd.hotplug $(1)/etc/hotplug.d/iface/20-ntpd
endef

define Package/ntpsecd/postinst
#!/bin/sh
[ -L "$${IPKG_INSTROOT}/usr/sbin/ntpd" ] && rm -f "$${IPKG_INSTROOT}/usr/sbin/ntpd"
exit 0
endef

define Package/ntpsecd/postrm
#!/bin/sh
/bin/busybox ntpd -h 2>&1 | grep -q BusyBox && ln -sf ../../bin/busybox /usr/sbin/ntpd
exit 0
endef

define Package/ntpsecdate/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/util/ntpdate $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/ntpdate.init $(1)/etc/init.d/ntpdate
endef

define Package/ntpsecdig/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/ntpdig $(1)/usr/bin/
endef

define Package/ntpseckeygen/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/ntpkeygen $(1)/usr/bin/
endef

define Package/ntpsec-utils/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/ntp{frob,q} $(1)/usr/bin/
endef

$(eval $(call BuildPackage,ntpsecd))
$(eval $(call BuildPackage,ntpsecdate))
$(eval $(call BuildPackage,ntpsecdig))
$(eval $(call BuildPackage,ntpseckeygen))
$(eval $(call BuildPackage,ntpsec-utils))
