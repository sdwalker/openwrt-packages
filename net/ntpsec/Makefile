#
# Copyright (C) 2016-2017 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ntpsec
#PKG_VERSION:=0.9.5-1
PKG_SOURCE_DATE:=2018-10-17
PKG_VERSION:=1.1.2-$(PKG_SOURCE_DATE)
PKG_RELEASE:=1

#PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
#PKG_SOURCE_URL:=ftp://ftp.ntpsec.org/pub/releases/
#PKG_MD5SUM:=f067bf8a6845dd8d301735ca733662c3

PKG_REV:=dfee955e911332f5dbc7f34c2e3fd3d6777b3597
PKG_SOURCE_URL:=https://gitlab.com/NTPsec/ntpsec.git
PKG_MIRROR_HASH:=223eca2c200442d1af308bf6444be3fe3f70573b7917f373053ec448b8562135
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_BUILD_DEPENDS:=pps-tools

PKG_CONFIG_DEPENDS:=CONFIG_NTPSEC_SECCOMP

include $(INCLUDE_DIR)/package.mk
include ../../lang/python/python-package.mk

define Package/ntpsecd/Default
  SUBMENU:=Time Synchronization
  SECTION:=net
  CATEGORY:=Network
  TITLE:=NTPsec
  URL:=https://www.ntpsec.org/
  DEPENDS:=+kmod-pps
endef

define Package/ntpsecd/Default/description
 A secure, hardened, and improved implementation of Network Time Protocol
 derived from NTP Classic, Dave Mills's original.
endef

define Package/ntpsecd
$(call Package/ntpsecd/Default)
  TITLE+= server
  USERID:=ntp=123:ntp=123
  DEPENDS+= +libcap +libopenssl +NTPSEC_SECCOMP:libseccomp
endef

define Package/ntpsecd/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains the ntpd server.
endef

define Package/ntpsecd/config
  source "$(SOURCE)/Config.in"
endef

define Package/ntpsecdate
$(call Package/ntpsecd/Default)
  TITLE+=date
  DEPENDS+= +ntpsec-utils
endef

define Package/ntpsecdate/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains ntpdate.
endef

define Package/ntpsecmon
$(call Package/ntpsecd/Default)
  TITLE+=mon
  DEPENDS+= +ntpsec-utils +python-ncurses
endef

define Package/ntpsecmon/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains ntpmon.
endef

define Package/ntpsec-python
$(call Package/ntpsecd/Default)
  TITLE+= Python
  DEPENDS+= +python-codecs +python-logging +python-openssl
endef

define Package/ntpsec-python/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains the Python libraries.
endef

define Package/ntpsec-utils
$(call Package/ntpsecd/Default)
  TITLE+= utilities
  DEPENDS+= +ntpsec-python
endef

define Package/ntpsec-utils/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains ntpdig, ntpfrob, ntpkeygen, ntpmon and ntpq.
endef

define Package/ntpsecviz
$(call Package/ntpsecd/Default)
  TITLE+=viz
  DEPENDS+= +gnuplot +ntpsec-python
endef

define Package/ntpsecviz/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains ntpviz.
endef

define Package/ntpsecd/conffiles
/etc/ntp.conf
endef

TARGET_CPPFLAGS += -I$(PYTHON_INC_DIR)

define Build/Compile
	cd $(PKG_BUILD_DIR) && \
	$(HOST_CONFIGURE_VARS) \
	./waf configure && \
	./waf build_host && \
	./waf install_host && \
	cd $(PKG_BUILD_DIR) && \
	$(CONFIGURE_VARS) \
	./waf configure \
		--build-epoch="$(SOURCE_DATE_EPOCH)" \
		--cross-compiler="$(TARGET_CC)" \
		--disable-manpage \
		--disable-mdns-registration \
$(if $(CONFIG_NTPSEC_SECCOMP), \
		--enable-seccomp \
) \
		--prefix=/usr \
		--refclock="shm" && \
	./waf build_main && \
	./waf install_main \
		--destdir="$(PKG_INSTALL_DIR)"
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
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/attic/ntpdate $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/ntpdate.init $(1)/etc/init.d/ntpdate
endef

define Package/ntpsecmon/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/ntpmon $(1)/usr/bin/
endef

define Package/ntpsec-python/install
	$(INSTALL_DIR) $(1)$(PYTHON_PKG_DIR)
	$(CP) \
	    $(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/* \
	    $(1)$(PYTHON_PKG_DIR)
	$(CP) \
	    $(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/ntp/ntpc \
	    $(1)$(PYTHON_PKG_DIR)/ntp/ntpc.so
	$(RM) $(1)$(PYTHON_PKG_DIR)/ntp/ntpc
endef

define Package/ntpsec-utils/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) \
		$(PKG_INSTALL_DIR)/usr/bin/ntp{dig,frob,keygen,q} \
		$(1)/usr/bin/
endef

define Package/ntpsecviz/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/ntpviz $(1)/usr/bin/
endef

$(eval $(call BuildPackage,ntpsecd))
$(eval $(call BuildPackage,ntpsecdate))
$(eval $(call BuildPackage,ntpsecmon))
$(eval $(call BuildPackage,ntpsec-python))
$(eval $(call BuildPackage,ntpsec-utils))
$(eval $(call BuildPackage,ntpsecviz))
