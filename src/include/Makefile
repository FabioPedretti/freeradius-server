#
# Makefile
#
# Version:	$Id$
#

HEADERS	= conf.h conffile.h detail.h dhcp.h event.h features.h hash.h heap.h \
	ident.h libradius.h md4.h md5.h missing.h modcall.h modules.h \
	packet.h rad_assert.h radius.h radiusd.h radpaths.h \
	radutmp.h realms.h sha1.h stats.h sysutmp.h token.h \
	udpfromto.h vmps.h vqp.h base64.h

include ../../Make.inc
.PHONY: all clean distclean install

.PHONY: all clean distclean install reconfig
all: radpaths.h features.h missing.h tls.h

radpaths.h: build-radpaths-h
	@/bin/sh ./build-radpaths-h

#
# This is hacky, feature defines should go away or be set with configure.in
#
autoconf.sed: autoconf.h
	@grep ^#define $< | sed 's,/\*\*/,1,;' | awk '{print "\
	s,#[[:blank:]]*ifdef[[:blank:]]*" $$2 ",#if "$$3 ",g;\
	s,#[[:blank:]]*ifndef[[:blank:]]*" $$2 ",#if !"$$3 ",g;\
	s,defined(" $$2 ")," $$3 ",g;\
	s," $$2 ","$$3 ",g;"}' > $@

features.h: features-h autoconf.h
	@cp $< $@
	@grep "^#define[[:blank:]]\{1,\}WITH_" autoconf.h >> features.h
	
missing.h: missing-h autoconf.sed
	 @sed -f autoconf.sed < $< > $@

tls.h: tls-h autoconf.sed
	@sed -f autoconf.sed < $< > $@

distclean: clean
	@rm -f radpaths.h

reconfig clean:
	@rm -f features.h missing.h tls.h
	@rm -f autoconf.sed

install:
	$(INSTALL) -d -m 755 $(R)$(includedir)/freeradius
	for i in $(HEADERS); do \
		sed 's/^#include <freeradius-devel/#include <freeradius/' $$i > .inst.$$$$ ; \
		$(INSTALL) -m 644 .inst.$$$$   $(R)$(includedir)/freeradius/$$i; \
		rm -f .inst.$$$$ ; \
	done
