include ../../Makefile.omd

NAME = icinga2
VERSION = 0.0.2
DIR = $(NAME)-$(VERSION)

.PHONY: skel

# Configure options for Icinga. Since we want to compile
# as non-root, we use our own user and group for compiling.
# All files will be packaged as user 'root' later anyway.
CONFIGUREOPTS = \
    --prefix=$(OMD_ROOT)

build:
	tar xzf $(DIR).tar.gz
	cd $(DIR) ; ./configure $(CONFIGUREOPTS)
	$(MAKE) -j 2 -C $(DIR) all

install:
	$(MAKE) DESTDIR=$(DESTDIR) -C $(DIR) install
	rm -rf $(DESTDIR)$(OMD_ROOT)/etc
	rm -rf $(DESTDIR)$(OMD_ROOT)/var

skel:
	mkdir -p $(SKEL)/etc/icinga2/itl
	cp -rp $(DIR)/itl/*.conf $(SKEL)/etc/icinga2/itl/

clean:
	rm -rf $(DIR)

upstream:
	git rm *.gz
	wget "https://git.icinga.org/?p=icinga2.git;a=snapshot;refs/heads/master;hb=master;sf=tgz" -O icinga2.tar.gz
	tar zxf icinga2.tar.gz
	cd icinga2 && ./autogen.sh && make dist
	rm -rf icinga2.tar.gz
	mv icinga2/*.gz .
	VERSION=$$(ls *.gz | sed -e 's/^icinga2-//' -e 's/.tar.gz$$//'); sed -i Makefile -e 's/^VERSION = .*/VERSION = '$$VERSION'/g'
	git add *.gz
	git commit -a -m "icinga2: update to latest $$(ls -1 *.gz)"
	rm -rf icinga2
