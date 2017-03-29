#
# Copyright (C) 2014-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=mwlwifi
PKG_VERSION:=10.3.3.0-20170210
PKG_RELEASE=2

PKG_LICENSE:=ISC
PKG_LICENSE_FILES:=

#PKG_SOURCE_URL:=https://github.com/marvell-wireless/mwlwifi.git
#PKG_SOURCE_PROTO:=git
#PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
#PKG_SOURCE_VERSION:=e38991714d951ca5ea16133ffed1c9f547f7ebfd
#PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.xz

PKG_MAINTAINER:=panda-mute <wxuzju@gmail.com>
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/mwlwifi-pcie
  SUBMENU:=Wireless Drivers
  TITLE:=Marvell 88W8864/88W8897/88W8964/88W8997 Wireless driver for PCIe Wireless cards
  DEPENDS:=+kmod-mac80211 +@DRIVER_11N_SUPPORT +@DRIVER_11AC_SUPPORT +@DRIVER_11W_SUPPORT @PCI_SUPPORT
  FILES:=$(PKG_BUILD_DIR)/mwlwifi_comm.ko $(PKG_BUILD_DIR)/mwlwifi_pcie.ko
  AUTOLOAD:=$(call AutoLoad,50,mac80211 mwlwifi_comm mwlwifi_pcie)
endef

define KernelPackage/mwlwifi-sdio
  SUBMENU:=Wireless Drivers
  TITLE:=Marvell 88W8864/88W8897/88W8964/88W8997 Wireless driver for PCIe Wireless cards
  DEPENDS:=+kmod-mac80211 +@DRIVER_11N_SUPPORT +@DRIVER_11AC_SUPPORT +@DRIVER_11W_SUPPORT
  FILES:=$(PKG_BUILD_DIR)/mwlwifi_comm.ko $(PKG_BUILD_DIR)/mwlwifi_sdio.ko
  AUTOLOAD:=$(call AutoLoad,50,mac80211 mwlwifi_comm mwlwifi_sdio)
endef

NOSTDINC_FLAGS = \
	-I$(PKG_BUILD_DIR) \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-include backport/backport.h

define KernelPackage/mwlwifi-pcie/config
	select mwlwifi-pcie if (TARGET_mvebu || TARGET_mvebu64)
endef

define KernelPackage/mwlwifi-sdio/config
	select mwlwifi-sdio if (TARGET_mvebu || TARGET_mvebu64)
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./src/* $(PKG_BUILD_DIR)/
	#$(Build/Patch)
endef

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		ARCH="$(LINUX_KARCH)" \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		SUBDIRS="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		modules
endef

define KernelPackage/mwlwifi-pcie/install
	$(INSTALL_DIR) $(1)/lib/firmware
	$(INSTALL_DIR) $(1)/lib/firmware/mwlwifi
	#$(CP) $(PKG_BUILD_DIR)/bin/firmware/88W8864.bin $(1)/lib/firmware/mwlwifi/88W8864_pcie.bin
	#$(CP) $(PKG_BUILD_DIR)/bin/firmware/88W8897.bin $(1)/lib/firmware/mwlwifi/88W8897_pcie.bin
	#$(CP) $(PKG_BUILD_DIR)/bin/firmware/88W8964.bin $(1)/lib/firmware/mwlwifi/88W8964_pcie.bin
	$(CP) $(PKG_BUILD_DIR)/bin/firmware/88W8997_pcie.bin $(1)/lib/firmware/mwlwifi/
	#$(CP) $(PKG_BUILD_DIR)/bin/firmware/Marvell_license.txt $(1)/lib/firmware/mwlwifi/
endef

define KernelPackage/mwlwifi-sdio/install
	$(INSTALL_DIR) $(1)/lib/firmware
	$(INSTALL_DIR) $(1)/lib/firmware/mwlwifi
	$(CP) $(PKG_BUILD_DIR)/bin/firmware/88W8997_sdio.bin $(1)/lib/firmware/mwlwifi/
	$(CP) $(PKG_BUILD_DIR)/bin/firmware/Marvell_license.txt $(1)/lib/firmware/mwlwifi/
endef

$(eval $(call KernelPackage,mwlwifi-pcie))
$(eval $(call KernelPackage,mwlwifi-sdio))
