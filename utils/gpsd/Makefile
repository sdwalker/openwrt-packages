#
# Copyright (C) 2006-2017 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=gpsd
#PKG_VERSION:=3.18.1
PKG_SOURCE_DATE:=2018-12-22
PKG_VERSION:=3.19-$(PKG_SOURCE_DATE)
PKG_RELEASE:=1

#PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
#PKG_SOURCE_URL:=@SAVANNAH/$(PKG_NAME)
#PKG_HASH:=5cb1e6d880ec9a52c62492dd0e3d77451b7c7ad625895bd652f6354215aec23e

PKG_REV:=e4779bf305a270380f4a33be77fb6200deb11517
PKG_SOURCE_URL:=git://git.sv.gnu.org/gpsd.git
PKG_MIRROR_HASH:=69fa9a12d88269b4e313b97d2f0d76d28c3b9c9ef87a8717cb4bd7b7a6e9418d
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_MAINTAINER:=Pushpal Sidhu <psidhu.devel@gmail.com>
PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=COPYING

PKG_BUILD_DEPENDS:=ncurses libusb pps-tools python
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/scons.mk
include ../../lang/python/python-package.mk

define Package/gpsd/Default
  DEPENDS+= +kmod-pps-ldisc +libusb-1.0
  URL:=http://catb.org/gpsd/
endef

define Package/gpsd/Default/description
  gpsd is a userland daemon acting as a translator between GPS and AIS receivers
  and their clients. gpsd listens on port 2947 for clients requesting
  position/time/velocity information.  The receivers are expected to generate
  position information in a well-known format -- as NMEA-0183 sentences, SiRF
  binary, Rockwell binary, Garmin binary format, or other vendor binary
  protocols.  gpsd takes this information from the GPS and translates it into
  something uniform and easier to understand for clients.
endef

define Package/gpsd
  $(call Package/gpsd/Default)
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=An interface daemon for GPS receivers
  VARIANT:=full
endef

define Package/gpsd/conffiles
/etc/config/gpsd
endef

define Package/gpsd/description
  $(call Package/gpsd/Default/description)
  This package contains the GPS daemon.
endef

define Package/gpsd-minimal
  $(call Package/gpsd/Default)
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=An interface daemon for GPS receivers (minimal)
  VARIANT:=minimal
endef

define Package/gpsd-minimal/conffiles
$(call Package/gpsd/conffiles)
endef

define Package/gpsd-minimal/description
  $(call Package/gpsd/Default/description)
  This package contains the GPS daemon in the minimal configuration.
endef

define Package/gpsd-timeservice
  $(call Package/gpsd/Default)
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=An interface daemon for GPS receivers (timeservice)
  VARIANT:=timeservice
endef

define Package/gpsd-timeservice/conffiles
$(call Package/gpsd/conffiles)
endef

define Package/gpsd-timeservice/description
  $(call Package/gpsd/Default/description)
  This package contains the GPS daemon in the time-service configuration.
endef

define Package/gpsd-clients
  $(call Package/gpsd/Default)
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS+= +libncurses
  TITLE:=GPS tools and clients
  VARIANT:=full
endef

define Package/gpsd-clients/description
  $(call Package/gpsd/Default/description)
  This package contains auxiliary tools and example clients for monitoring and
  testing the GPS daemon.
endef

define Package/gpsd-clients-minimal
  $(call Package/gpsd/Default)
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS+= +libncurses
  TITLE:=GPS tools and clients (minimal)
  VARIANT:=minimal
endef

define Package/gpsd-clients-minimal/description
  $(call Package/gpsd/Default/description)
  This package contains auxiliary tools and example clients for monitoring and
  testing the GPS daemon in the minimal configuration.
endef

define Package/gpsd-clients-timeservice
  $(call Package/gpsd/Default)
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS+= +libncurses
  TITLE:=GPS tools and clients (timeservice)
  VARIANT:=timeservice
endef

define Package/gpsd-clients-timeservice/description
  $(call Package/gpsd/Default/description)
  This package contains auxiliary tools and example clients for monitoring and
  testing the GPS daemon in the time-service configuration.
endef

define Package/gpsd-python
  $(call Package/gpsd/Default)
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS+= +python-light
  TITLE:=GPS Python bindings
  VARIANT:=full
endef

define Package/gpsd-python/description
  $(call Package/gpsd/Default/description)
endef

define Package/gpsd-python-minimal
  $(call Package/gpsd/Default)
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS+= +python-light
  TITLE:=GPS Python bindings (minimal)
  VARIANT:=minimal
endef

define Package/gpsd-python-minimal/description
  $(call Package/gpsd/Default/description)
endef

define Package/gpsd-python-timeservice
  $(call Package/gpsd/Default)
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS+= +python-light
  TITLE:=GPS Python bindings (timeservice)
  VARIANT:=timeservice
endef

define Package/gpsd-python-timeservice/description
  $(call Package/gpsd/Default/description)
endef

define Package/libgps
  $(call Package/gpsd/Default)
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=C service library for communicating with the GPS daemon
  VARIANT:=full
endef

define Package/libgps/description
  $(call Package/gpsd/Default/description)
  This package contains the libgps library.
endef

define Package/libgps-minimal
  $(call Package/gpsd/Default)
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=C service library for communicating with the GPS daemon (minimal)
  VARIANT:=minimal
endef

define Package/libgps-minimal/description
  $(call Package/gpsd/Default/description)
  This package contains the libgps library in the minimal configuration.
endef

define Package/libgps-timeservice
  $(call Package/gpsd/Default)
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=C service library for communicating with the GPS daemon (timeservice)
  VARIANT:=timeservice
endef

define Package/libgps-timeservice/description
  $(call Package/gpsd/Default/description)
  This package contains the libgps library in the time-service configuration.
endef

TARGET_CFLAGS += \
	-D_GNU_SOURCE \
	-I$(PYTHON_INC_DIR)

SCONS_VARS += \
	LINKFLAGS="$(TARGET_LDFLAGS)"

ifeq ($(BUILD_VARIANT),full)
SCONS_OPTIONS += \
	dbus_export=no \
	tsip=no \
	fv18=no \
	tripmate=no \
	earthmate=no \
	itrax=no \
	navcom=no \
	ubx=no \
	evermore=no \
	ntrip=no \
	libgpsmm=no \
	libQgpsmm=no \
	bluez=no \
	nostrip=yes \
	python=no \
	implicit_link=no \
	chrpath=no \
	manbuild=no \
	target="$(TARGET_CROSS:-=)"
endif

ifeq ($(BUILD_VARIANT),minimal)
SCONS_OPTIONS += \
	minimal=yes \
	control_socket=yes \
	controlsend=yes \
	ipv6=yes \
	ncurses=yes \
	netfeed=yes \
	nmea0183=yes \
	nostrip=yes \
	ntp=yes \
	ntpshm=yes \
	oscillator=yes \
	passthrough=yes \
	pps=yes \
	reconfigure=yes \
	shared=yes \
	shm_export=yes \
	socket_export=yes \
	target="$(TARGET_CROSS:-=)" \
	usb=yes
endif

ifeq ($(BUILD_VARIANT),timeservice)
SCONS_OPTIONS += \
	gpsdclients=yes \
	mtk3301=no \
	nmea0183=yes \
	nostrip=yes \
	passthrough=yes \
	python=yes \
	python_libdir="/usr/lib" \
	shared=yes \
	target="$(TARGET_CROSS:-=)" \
	target_python="$(PYTHON)" \
	timeservice=yes \
	ublox=no \
	usb=yes
endif

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/gps.h $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libgps.so* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libgps.pc \
		$(1)/usr/lib/pkgconfig/
endef

define Package/gpsd/install
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/gpsd.config $(1)/etc/config/gpsd
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/gpsd.init $(1)/etc/init.d/gpsd
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/gpsd $(1)/usr/sbin/
endef

Package/gpsd-minimal/install = $(Package/gpsd/install)
Package/gpsd-timeservice/install = $(Package/gpsd/install)

define Package/gpsd-clients/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) \
		$(PKG_INSTALL_DIR)/usr/bin/cgps \
		$(PKG_INSTALL_DIR)/usr/bin/gps{2udp,ctl,decode,mon,pipe} \
		$(PKG_INSTALL_DIR)/usr/bin/gpxlogger \
		$(PKG_INSTALL_DIR)/usr/bin/lcdgps \
		$(PKG_INSTALL_DIR)/usr/bin/ntpshmmon \
		$(PKG_INSTALL_DIR)/usr/bin/ppscheck \
		$(1)/usr/bin/
endef

Package/gpsd-clients-minimal/install = $(Package/gpsd-clients/install)
Package/gpsd-clients-timeservice/install = $(Package/gpsd-clients/install)

define Package/gpsd-python/install
	$(INSTALL_DIR) $(1)$(PYTHON_PKG_DIR)/gps
	$(CP) \
	    $(PKG_INSTALL_DIR)/usr/lib/gps/* \
	    $(1)$(PYTHON_PKG_DIR)/gps/
endef

Package/gpsd-python-minimal/install = $(Package/gpsd-python/install)
Package/gpsd-python-timeservice/install = $(Package/gpsd-python/install)

define Package/libgps/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libgps.so.* $(1)/usr/lib/
endef

Package/libgps-minimal/install = $(Package/libgps/install)
Package/libgps-timeservice/install = $(Package/libgps/install)

$(eval $(call BuildPackage,gpsd))
$(eval $(call BuildPackage,gpsd-minimal))
$(eval $(call BuildPackage,gpsd-timeservice))
$(eval $(call BuildPackage,gpsd-clients))
$(eval $(call BuildPackage,gpsd-clients-minimal))
$(eval $(call BuildPackage,gpsd-clients-timeservice))
$(eval $(call BuildPackage,gpsd-python))
$(eval $(call BuildPackage,gpsd-python-minimal))
$(eval $(call BuildPackage,gpsd-python-timeservice))
$(eval $(call BuildPackage,libgps))
$(eval $(call BuildPackage,libgps-minimal))
$(eval $(call BuildPackage,libgps-timeservice))
