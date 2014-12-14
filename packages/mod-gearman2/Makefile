include ../../Makefile.omd

NAME     = mod_gearman2
VERSION  = 2.0.0
GEARMAND = $(shell grep "^VERSION " ../gearmand/Makefile | awk '{ print $$3 }')
DIR      = $(NAME)-$(VERSION)

.PHONY: skel

build:
	tar xzf $(DIR).tar.gz
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
