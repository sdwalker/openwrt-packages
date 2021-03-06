#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=pping
PKG_SOURCE_DATE:=2018-05-15
PKG_VERSION:=$(PKG_SOURCE_DATE)
PKG_RELEASE:=1

PKG_REV:=777eb72fd9b748b4bb628ef97b7fff19b751f1fd
PKG_SOURCE_URL:=https://github.com/pollere/pping.git
PKG_MIRROR_HASH:=1e60cded499a614598c180970c9eb952e41ecc170ae12aa63f07c62313e2e0e3
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/pping
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libtins
  TITLE:=pping (pollere passive ping)
  URL:=http://pollere.net/pping.html
endef

define Package/pping/description
  pping is a linux/macOS/BSD command line tool to measure network latency via
  passive monitoring of active connections. Unlike ping, pping doesn't inject
  traffic to determine RTT (Round-Trip Time) -- it reports the per-packet RTT
  experienced by normal application traffic. Unlike transport state monitoring
  tools like ss which can only measure RTT at the sending endpoint, pping can
  measure RTT at the sender, receiver or anywhere on a connection's path (for
  example, an OpenWrt home border router could easily monitor the RTT of all
  traffic to and from the Internet).
endef

define Package/pping/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_BUILD_DIR)/pping $(1)/usr/bin/
endef

$(eval $(call BuildPackage,pping))
