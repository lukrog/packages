include $(TOPDIR)/rules.mk

PKG_NAME:=runit
PKG_VERSION:=2.1.2
PKG_RELEASE:=1

#PKG_BUILD_DIR:=$(BUILD_DIR)/runit-$(PKG_VERSION)
PKG_SOURCE_PROTO:=git
#PKG_SOURCE:=runit-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=ssh://git@ph.marketlab.pl/diffusion/113/runit.git
PKG_SOURCE_VERSION:=cf01a5590157772d885b47747d1df0e15003b192
#PKG_HASH:=6fd0160cb0cf1207de4e66754b6d39750cff14bb0aa66ab49490992c0c47ba18

include $(INCLUDE_DIR)/package.mk

define Package/runit
	SECTION:=admin
	CATEGORY:=Administration
	TITLE:=runit process supervisor
	DEPENDS:=+libc
	#DESCRIPTION:=This variable is obsolete. use the Package/name/description define instead!
	URL:=http://smarden.org/runit/
endef

define Package/runit/description
	runit is a process supervisor similar to djb's daemontools. It contains tools for
	supervision, log collection and rotation.
endef

define Build/Prepare
	tar -xf $(DL_DIR)/runit-$(PKG_VERSION).tar.xz --strip-components=1  -C $(PKG_BUILD_DIR)
#echo $(.VARIABLES)
#echo $(PWD)
#cp $(PKG_BUILD_DIR)/admin/runit-2.1.2/src $(PKG_BUILD_DIR)/
#rm $(PKG_BUILD_DIR)/admin/runit-2.1.2/src/conf-cc
	echo "$(TARGET_CC) $(TARGET_CFLAGS)" > $(PKG_BUILD_DIR)/src/conf-cc
	echo "$(TARGET_CC) $(TARGET_LDFLAGS)" > $(PKG_BUILD_DIR)/src/conf-ld
	sed -i "s/'ar/'$(TARGET_AR)/;s/'ranlib/'$(TARGET_RANLIB)/" $(PKG_BUILD_DIR)/src/print-ar.sh
#line below prevents removing 'try' executables that determine header config macros. Usefull when working with new OS
#sed -i '16d' $(PKG_BUILD_DIR)/admin/runit-2.1.2/src/choose.sh



#echo 'task goes here' | cat - todo.txt > temp && mv temp todo.txt
endef

define Build/Compile
#tryflock 		test returned 0
#trymkffo 		test returned 255	!!!!
#trypoll 		test returned 111	!!!!
#trysgact 		test returned 0
#trysgprm 		test returned 0
#tryshsgr 		test returned 1		!!!!
#tryulong64 	test returned 1		!!!!
#trywaitp 		test returned 0

#Makefile script wants to test runtime results for (tryshsgr, tryulong64, trypoll).
#For all of them the result is error on tested device
#So we can hardcode results of all tests

#Tests depend on compile and load rules that are generated. We generate them first
	(cd $(PKG_BUILD_DIR)/src ; make compile load chkshsgr choose)

#Here we hardcode results of runtime tests
	cp $(PKG_BUILD_DIR)/src/uint64.h1 $(PKG_BUILD_DIR)/src/uint64.h
	cp $(PKG_BUILD_DIR)/src/hasshsgr.h1 $(PKG_BUILD_DIR)/src/hasshsgr.h
	cp $(PKG_BUILD_DIR)/src/iopause.h1 $(PKG_BUILD_DIR)/src/iopause.h
	cp $(PKG_BUILD_DIR)/src/hasshsgr.h1 $(PKG_BUILD_DIR)/src/hasshsgr.h
#Now we can build and make script will not override .h files
	(cd $(PKG_BUILD_DIR)/src ; make)

endef


# define Build/Install
# 	$(DL_DIR)
# endef
define Package/runit/install
	$(INSTALL_DIR) $(1)/bin
	$(CP) $(PKG_BUILD_DIR)/src/chpst $(1)/bin/
	$(CP) $(PKG_BUILD_DIR)/src/runit  $(1)/bin/
	$(CP) $(PKG_BUILD_DIR)/src/runit-init $(1)/bin/
	$(CP) $(PKG_BUILD_DIR)/src/runsv  $(1)/bin/
	$(CP) $(PKG_BUILD_DIR)/src/runsvchdir $(1)/bin/
	$(CP) $(PKG_BUILD_DIR)/src/runsvdir  $(1)/bin/
	$(CP) $(PKG_BUILD_DIR)/src/sv  $(1)/bin/
	$(CP) $(PKG_BUILD_DIR)/src/svlogd  $(1)/bin/
	$(CP) $(PKG_BUILD_DIR)/src/utmpset  $(1)/bin/
endef

$(eval $(call BuildPackage,runit))


