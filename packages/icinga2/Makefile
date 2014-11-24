include ../../Makefile.omd

NAME = icinga2
VERSION = 2.2.0
DIR = $(NAME)-$(VERSION)

.PHONY: skel

# Configure options for Icinga. Since we want to compile
# as non-root, we use our own user and group for compiling.
# All files will be packaged as user 'root' later anyway.
CONFIGUREOPTS = \
    -DCMAKE_INSTALL_PREFIX=$(OMD_ROOT) \
    -DINSTALL_SBINDIR=$(OMD_ROOT)/bin \
    -DICINGA2_WITH_MYSQL=OFF \
    -DICINGA2_WITH_PGSQL=OFF

build:
	tar xzf $(DIR).tar.gz
	mkdir -p $(DIR)/build
	cd $(DIR)/build; cmake .. $(CONFIGUREOPTS)
	cd $(DIR)/build; make
	#$(MAKE) -j 2 -C $(DIR) all

install:
	$(MAKE) DESTDIR=$(DESTDIR) -C $(DIR)/build install
	rm -rf $(DESTDIR)$(OMD_ROOT)/etc
	rm -rf $(DESTDIR)$(OMD_ROOT)/var

skel:
	mkdir -p $(SKEL)/etc/icinga2/itl
	cp -rp $(DIR)/itl/*.conf $(SKEL)/etc/icinga2/itl/

clean:
	rm -rf $(DIR)
