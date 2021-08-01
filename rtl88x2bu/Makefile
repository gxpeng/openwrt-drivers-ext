include $(TOPDIR)/rules.mk

PKG_NAME:=rtl88x2bu
PKG_VERSION:=5.8.7.1
PKG_RELEASE=1

PKG_LICENSE:=GPLv3
PKG_LICENSE_FILES:=LICENSE

# PKG_SOURCE_URL:=https://github.com/cilynx/rtl88x2bu.git
# PKG_SOURCE_PROTO:=git
# PKG_SOURCE_DATE:=2021-07-06
# PKG_SOURCE_VERSION:=1f859429e8bc42d888fd88c1aecd512c4386f2b2
# PKG_MIRROR_HASH:=80f6c672e4d10f0a429623b3680911580b2bc5813b44c9ccb255f697058c3171

PKG_MAINTAINER:=[GXPENG] Project OpenWrt
PKG_BUILD_PARALLEL:=1


STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl88x2bu
  SUBMENU:=Wireless Drivers
  TITLE:=Realtek 8812BU/8822BU support
  DEPENDS:=+kmod-cfg80211 +kmod-usb-core +@DRIVER_11N_SUPPORT +@DRIVER_11AC_SUPPORT
  FILES:=$(PKG_BUILD_DIR)/rtl88x2bu.ko
  AUTOLOAD:=$(call AutoProbe,rtl88x2bu)
  PROVIDES:=kmod-rtl88x2bu
endef

define KernelPackage/rtl88x2bu/description
 Kernel module for rtl8812bu/8822bu wireless module.
endef

NOSTDINC_FLAGS = \
	-I$(PKG_BUILD_DIR) \
	-I$(PKG_BUILD_DIR)/include \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-include backport/backport.h

EXTRA_CFLAGS:= \
	-DRTW_SINGLE_WIPHY \
	-DRTW_USE_CFG80211_STA_EVENT \
	-DCONFIG_RADIO_WORK \
	-DCONFIG_IOCTL_CFG80211 \
	-DCONFIG_CONCURRENT_MODE \
	-DBUILD_OPENWRT

EXTRA_KCONFIG:= \
	CONFIG_POWER_SAVING=n \
	CONFIG_RTW_DEBUG=y \
	CONFIG_RTL8822BU=m \
	USER_MODULE_NAME=rtl88x2bu

MAKE_OPTS:= \
	$(KERNEL_MAKE_FLAGS) \
	M="$(PKG_BUILD_DIR)" \
	NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
	USER_EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
	$(EXTRA_KCONFIG)

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		$(MAKE_OPTS) \
		modules
endef

$(eval $(call KernelPackage,rtl88x2bu))
