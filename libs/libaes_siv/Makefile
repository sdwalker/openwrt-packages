#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libaes_siv
#PKG_VERSION:=1.0.0
PKG_SOURCE_DATE:=2019-02-15
PKG_VERSION:=1.0.0-$(PKG_SOURCE_DATE)
PKG_RELEASE:=1

PKG_REV:=6352fbc9450530c85a654c509267f777eea28301
PKG_SOURCE_URL:=https://github.com/dfoxfranke/libaes_siv.git
PKG_MIRROR_HASH:=6c2fbdae19fac160ac977a360a4d3c9a9ede34ea36837d6c7ddeafbef26959da
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_REV)

CMAKE_INSTALL:=1

PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=COPYING

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/libaes_siv
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libopenssl
  TITLE:=An RFC5297-compliant C implementation of AES-SIV
endef

define Package/libaes_siv/description
  This is an RFC5297-compliant C implementation of AES-SIV, written by Daniel
  Franke on behalf of Akamai Technologies and published under the Apache License
  (v2.0). It uses OpenSSL for the underlying AES and CMAC implementations and
  follows a similar interface style.
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/aes_siv.h $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libaes_siv.{a,so*} $(1)/usr/lib/
endef

define Package/libaes_siv/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libaes_siv.so.* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libaes_siv))
