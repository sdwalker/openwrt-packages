#
# Copyright (C) 2010-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=gnuplot
PKG_VERSION:=5.2.5
PKG_RELEASE:=1

PKG_INSTALL:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=@SF/gnuplot
PKG_HASH:=039db2cce62ddcfd31a6696fe576f4224b3bc3f919e66191dfe2cdb058475caa

PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/uclibc++.mk
include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

TARGET_LDFLAGS+= \
	-Wl,-rpath-link=$(STAGING_DIR)/usr/lib

CONFIGURE_VARS += \
	CXXFLAGS="$$$$CXXFLAGS -fno-rtti" \
	LIBS="-liconv -lpthread"

define Package/gnuplot
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:=+libncurses $(CXX_DEPENDS) +zlib +libpng +libfreetype +libgd \
	$(INTL_DEPENDS) $(ICONV_DEPENDS) +libpthread +libreadline
  TITLE:=A portable command-line driven graphing utility
  URL:=http://sourceforge.net/projects/gnuplot/
endef

define Package/gnuplot/description
  Gnuplot is a portable command-line driven graphing utility for linux, OS/2,
  MS Windows, OSX, VMS, and many other platforms.
endef

CONFIGURE_ARGS += \
	--without-x \
	--without-tutorial \
	--without-row-help \
	--disable-wxwidgets \
	--without-lua \
	--without-cairo

define Package/gnuplot/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/gnuplot $(1)/usr/bin/
endef

$(eval $(call BuildPackage,gnuplot))
