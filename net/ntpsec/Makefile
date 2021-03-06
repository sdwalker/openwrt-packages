#
# Copyright (C) 2016-2017 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ntpsec
#PKG_VERSION:=1.1.2
PKG_SOURCE_DATE:=2019-02-24
PKG_VERSION:=1.1.3-$(PKG_SOURCE_DATE)
PKG_RELEASE:=1

#PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
#PKG_SOURCE_URL:=ftp://ftp.ntpsec.org/pub/releases/
#PKG_HASH:=9dfaf1d791109160b3632a203bd75c784c54902442cea36983d5131b9f3b0111

PKG_REV:=9bb3234ceb79ccec3cdc1feb66006bb42b7a3b2f
PKG_SOURCE_URL:=https://gitlab.com/NTPsec/ntpsec.git
PKG_MIRROR_HASH:=ffc7e6bf61b4a5b99f6f0091f1e5ab9fd23628bafa01b972c108986b9772c53b
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_BUILD_DEPENDS:=pps-tools python python3

PKG_CONFIG_DEPENDS:=CONFIG_NTPSEC_PYTHON CONFIG_NTPSEC_SECCOMP

include $(INCLUDE_DIR)/package.mk
include ../../lang/python/python-package.mk
include ../../lang/python/python3-package.mk

define Package/ntpsecd/Default
  SUBMENU:=Time Synchronization
  SECTION:=net
  CATEGORY:=Network
  TITLE:=NTPsec
  URL:=https://www.ntpsec.org/
endef

define Package/ntpsecd/Default/description
 A secure, hardened, and improved implementation of Network Time Protocol
 derived from NTP Classic, Dave Mills's original.
endef

define Package/ntpsecd
$(call Package/ntpsecd/Default)
  TITLE+= server
  USERID:=ntp=123:ntp=123
  DEPENDS:=+kmod-pps +libcap +libopenssl +NTPSEC_SECCOMP:libseccomp
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
  DEPENDS:=+ntpsec-utils
endef

define Package/ntpsecdate/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains ntpdate.
endef

define Package/ntpsecmon
$(call Package/ntpsecd/Default)
  TITLE+=mon
  DEPENDS:=+ntpsec-utils +NTPSEC_PYTHON:python-ncurses +NTPSEC_PYTHON3:python3-ncurses
endef

define Package/ntpsecmon/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains ntpmon.
endef

define Package/ntpsec-python
$(call Package/ntpsecd/Default)
  TITLE+= Python
  DEPENDS:=+python-codecs +python-logging +python-openssl
endef

define Package/ntpsec-python/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains the Python libraries.
endef

define Package/ntpsec-python3
$(call Package/ntpsecd/Default)
  TITLE+= Python3
  DEPENDS:=+python3-codecs +python3-logging +python3-openssl
endef

define Package/ntpsec-python3/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains the Python3 libraries.
endef

define Package/ntpsec-utils
$(call Package/ntpsecd/Default)
  TITLE+= utilities
  DEPENDS:=+NTPSEC_PYTHON:ntpsec-python +NTPSEC_PYTHON3:ntpsec-python3
endef

define Package/ntpsec-utils/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains ntpdig, ntpfrob, ntpkeygen, ntpmon and ntpq.
endef

define Package/ntpsecviz
$(call Package/ntpsecd/Default)
  TITLE+=viz
  DEPENDS:=+gnuplot +NTPSEC_PYTHON:ntpsec-python +NTPSEC_PYTHON3:ntpsec-python3
endef

define Package/ntpsecviz/description
$(call Package/ntpsecd/Default/description)
 .
 This package contains ntpviz.
endef

define Package/ntpsecd/conffiles
/etc/ntp.conf
endef

ifeq ($(CONFIG_NTPSEC_PYTHON),y)
	TARGET_CPPFLAGS += \
		-I$(PYTHON_INC_DIR)
else
	TARGET_CPPFLAGS += \
		-I$(PYTHON3_INC_DIR)
endif

define Build/Compile
	cd $(PKG_BUILD_DIR) && \
	$(HOST_CONFIGURE_VARS) \
	./waf configure \
		--python=$(if $(CONFIG_NTPSEC_PYTHON),$(PYTHON),$(PYTHON3)) && \
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
		--python=$(if $(CONFIG_NTPSEC_PYTHON),$(PYTHON),$(PYTHON3)) \
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

define Package/ntpsec-python3/install
	$(INSTALL_DIR) $(1)$(PYTHON3_PKG_DIR)
	$(CP) \
	    $(PKG_INSTALL_DIR)$(PYTHON3_PKG_DIR)/* \
	    $(1)$(PYTHON3_PKG_DIR)
	$(CP) \
	    $(PKG_INSTALL_DIR)$(PYTHON3_PKG_DIR)/ntp/ntpc \
	    $(1)$(PYTHON3_PKG_DIR)/ntp/ntpc.so
	$(RM) $(1)$(PYTHON3_PKG_DIR)/ntp/ntpc
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
$(eval $(call BuildPackage,ntpsec-python3))
$(eval $(call BuildPackage,ntpsec-utils))
$(eval $(call BuildPackage,ntpsecviz))
