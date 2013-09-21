include ../../Makefile.omd

NAME = naemon
VERSION = master
DIR = $(NAME)-core-$(VERSION)

# Configure options for Naemon. Since we want to compile
# as non-root, we use our own user and group for compiling.
# All files will be packaged as user 'root' later anyway.
CONFIGUREOPTS = \
    --bindir=$(OMD_ROOT)/bin \
    --enable-event-broker \
    --with-naemon-user=$$(id -un) \
    --with-naemon-group=$$(id -gn) \
    --without-tests

build:
	tar xzf naemon-core-$(VERSION).tar.gz
	set -e ; for p in patches/*.dif ; do \
	    echo "applying $$p..." ; \
	    ( cd $(DIR) ; patch -p1 -b ) < $$p ; \
	done
	cd $(DIR) ; ./configure $(CONFIGUREOPTS)
	$(MAKE) -C $(DIR) all

install:
	mkdir -p $(DESTDIR)$(OMD_ROOT)/bin
	mkdir -p $(DESTDIR)$(OMD_ROOT)/lib
	cp $(DIR)/naemon/naemon $(DESTDIR)$(OMD_ROOT)/bin/naemon
	chmod 755 $(DESTDIR)$(OMD_ROOT)/bin/naemon

skel:

clean:
	rm -rf $(DIR)

upstream: clean
	rm -f naemon-core-master.tar.gz
	wget https://github.com/naemon/naemon-core/tarball/master -O tmp.tar.gz
	tar zxf tmp.tar.gz
	mv naemon-naemon-core-??????? naemon-core-master
	cd naemon-core-master && autoreconf -i
	tar cfz naemon-core-master.tar.gz naemon-core-master
	git add naemon-core-master.tar.gz
	rm -rf naemon-core-master tmp.tar.gz
