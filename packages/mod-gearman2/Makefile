include ../../Makefile.omd

NAME     = mod_gearman
VERSION  = master
GEARMAND = $(shell grep "^VERSION " ../gearmand/Makefile | awk '{ print $$3 }')
DIR      = mod_gearman-$(VERSION)

.PHONY: skel

build:
	tar xzf mod_gearman-$(VERSION).tar.gz
	cd $(DIR)/include && ln -sfn $(shell ls -d1 `pwd`/../naemon/naemon-core-master/naemon) .
	cd $(DIR) && \
	C_INCLUDE_PATH=$(shell pwd)/../naemon/naemon-core-master/ \
		./configure --prefix=$(OMD_ROOT) --with-gearman=$(shell pwd)/../gearmand/gearmand-${GEARMAND} && \
	$(MAKE) -j 1

install:
	mkdir -p $(DESTDIR)$(OMD_ROOT)/lib/mod_gearman
	cp $(DIR)/mod_gearman2.o $(DESTDIR)$(OMD_ROOT)/lib/mod_gearman/mod_gearman2.o

skel:

clean:
	rm -rf $(DIR)

upstream:
	rm -f mod-gearman-master.tar.gz
	wget https://github.com/sni/mod_gearman/tarball/master -O tmp.tar.gz
	tar zxf tmp.tar.gz
	mv sni-mod_gearman-??????? mod_gearman-master
	cd mod_gearman-master && ./autogen.sh
	tar cfz mod_gearman-master.tar.gz mod_gearman-master
	git add mod_gearman-master.tar.gz
	rm -rf mod_gearman-master tmp.tar.gz
