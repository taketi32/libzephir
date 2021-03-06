## Process this file with automake to produce Makefile.in

AM_LDFLAGS = $(LIBTOOL_LDFLAGS) $(GPROF_LDFLAGS)
AM_CFLAGS = $(DEBUG_CFLAGS) $(WARN_CFLAGS) $(NOWARN_CFLAGS) $(ERROR_CFLAGS) $(GPROF_CFLAGS)

lib_LTLIBRARIES = libzephir.la

libzephir_la_CFLAGS = $(AM_CFLAGS) $(PIE_FLAGS)
libzephir_la_SOURCES = scanner.re scanner.c zephir.c
libzephir_la_LIBADD = -ljson-c -lm

# The rules of thumb, when dealing with these values are:
# - Always increase the revision value.
# - Increase the current value whenever an interface has been added, removed or changed.
# - Increase the age value only if the changes made to the ABI are backward compatible.
# https://autotools.io/libtool/version.html
# The order for version-info is <current>:<revision>:<age>
libzephir_la_LDFLAGS = $(AM_LDFLAGS) -version-info 1:1:0

pkginclude_HEADERS = \
	xx.h \
	zephir.h \
	scanner.h \
	libzephir.h

scanner.c: $(srcdir)/scanner.re
	$(RE2C) @RE2C_FLAGS@ --output $@ $(srcdir)/scanner.re

zephir.c: base.c $(srcdir)/lemon
	$(srcdir)/lemon @LEMON_FLAGS@ zephir.lemon
	cat base.c >> zephir.c

lemon: lemon.c lempar.c
	$(CC) -w lemon.c -o $@

clean-local:
	-rm -f *.out \
		*.gcno \
		*.gcda \
		*.loT \
		lemon \
		scanner.c \
		zephir.c \
		libzephir.so
	-rm -rf .libs

maintainer-clean-local:
	-rm -f $(srcdir)/Makefile.in
	-rm -f $(top_srcdir)/Makefile.in
