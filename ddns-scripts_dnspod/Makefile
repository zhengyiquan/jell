include $(TOPDIR)/rules.mk

PKG_NAME:=ddns-scripts_dnspod
PKG_VERSION:=1.0.2
PKG_RELEASE:=

PKG_LICENSE:=GPLv2
PKG_MAINTAINER:=Small_5

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=IP Addresses and Names
	TITLE:=DDNS extension for Dnspod.com/Dnspod.cn
	PKGARCH:=all
	DEPENDS:=+ddns-scripts +wget-ssl +ca-bundle
endef

define Package/$(PKG_NAME)/description
	Dynamic DNS Client scripts extension for Dnspod.com/Dnspod.cn
endef

define Build/Configure
endef

define Build/Compile
	$(CP) ./*.sh $(PKG_BUILD_DIR)
endef

define Package/$(PKG_NAME)/preinst
	#!/bin/sh
	# if NOT run buildroot then stop service
	[ -z "$${IPKG_INSTROOT}" ] && /etc/init.d/ddns stop >/dev/null 2>&1
	exit 0 # suppress errors
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/lib/ddns
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/*.sh $(1)/usr/lib/ddns
endef

define Package/$(PKG_NAME)/postinst
	#!/bin/sh
	# remove old services file entries
	/bin/sed -i '/dnspod\.com/d' $${IPKG_INSTROOT}/etc/ddns/services >/dev/null 2>&1
	/bin/sed -i '/dnspod\.cn/d' $${IPKG_INSTROOT}/etc/ddns/services >/dev/null 2>&1
	/bin/sed -i '/dnspod\.com/d' $${IPKG_INSTROOT}/etc/ddns/services_ipv6 >/dev/null 2>&1
	/bin/sed -i '/dnspod\.cn/d' $${IPKG_INSTROOT}/etc/ddns/services_ipv6 >/dev/null 2>&1
	# and create new
	printf "%s\\t\\t%s\\n" '"dnspod.com"' '"update_dnspod_com.sh"' >> $${IPKG_INSTROOT}/etc/ddns/services
	printf "%s\\t\\t%s\\n" '"dnspod.cn"' '"update_dnspod_cn.sh"' >> $${IPKG_INSTROOT}/etc/ddns/services
	printf "%s\\t\\t%s\\n" '"dnspod.com"' '"update_dnspod_com.sh"' >> $${IPKG_INSTROOT}/etc/ddns/services_ipv6
	printf "%s\\t\\t%s\\n" '"dnspod.cn"' '"update_dnspod_cn.sh"' >> $${IPKG_INSTROOT}/etc/ddns/services_ipv6
	# on real system restart service if enabled
	[ -z "$${IPKG_INSTROOT}" ] && {
		/etc/init.d/ddns enabled && \
			/etc/init.d/ddns start >/dev/null 2>&1
	}
	exit 0 # suppress errors
endef

define Package/$(PKG_NAME)/prerm
	#!/bin/sh
	# if NOT run buildroot then stop service
	[ -z "$${IPKG_INSTROOT}" ] && /etc/init.d/ddns stop >/dev/null 2>&1
	# remove services file entries
	/bin/sed -i '/dnspod\.com/d' $${IPKG_INSTROOT}/etc/ddns/services >/dev/null 2>&1
	/bin/sed -i '/dnspod\.cn/d' $${IPKG_INSTROOT}/etc/ddns/services >/dev/null 2>&1
	/bin/sed -i '/dnspod\.com/d' $${IPKG_INSTROOT}/etc/ddns/services_ipv6 >/dev/null 2>&1
	/bin/sed -i '/dnspod\.cn/d' $${IPKG_INSTROOT}/etc/ddns/services_ipv6 >/dev/null 2>&1
	exit 0 # suppress errors
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
