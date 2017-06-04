#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libtins
PKG_VERSION:=3.5
PKG_RELEASE:=1

PKG_SOURCE:=v$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/mfontanini/libtins/archive
PKG_HASH:=1b0624b2eea3ce077a86f3abd3e625661760c4cfd21cd8f3d3cd3622229ff2cd

CMAKE_INSTALL:=1

PKG_LICENSE:=BSD-2-Clause
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/libtins
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libpcap +libstdcpp
  TITLE:=Packet crafting and sniffing library
  URL:=https://libtins.github.io/
endef

define Package/libtins/description
  libtins is a high-level, multiplatform C++ network packet sniffing and
  crafting library.

  Its main purpose is to provide the C++ developer an easy, efficient, platform
  and endianess-independent way to create tools which need to send, receive and
  manipulate specially crafted packets.
endef

CMAKE_OPTIONS += \
	-DCROSS_COMPILING=1 \
	-DLIBTINS_ENABLE_CXX11=1 \
	-DLIBTINS_ENABLE_WPA2=0

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/tins $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libtins.so* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libtins.pc \
		$(1)/usr/lib/pkgconfig/
endef

define Package/libtins/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libtins.so.* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libtins))
