DEFAULT_INCLUDES =				\
	-I$(srcdir)/../mruby-source/include	\
	-I$(srcdir)/../mruby-source/src

if WITH_MRUBY
noinst_LTLIBRARIES = libmruby.la

include sources.am

BUILT_SOURCES =

MRUBY_BUILD_DIR = $(abs_top_builddir)/vendor/mruby-build

BUILT_SOURCES += parse.c
libmruby_la_SOURCES += parse.c
parse.c: mruby-build.timestamp
	cp $(MRUBY_BUILD_DIR)/host/src/y.tab.c $@

BUILT_SOURCES += mrblib.c
libmruby_la_SOURCES += mrblib.c
mrblib.c: mruby-build.timestamp
	cp $(MRUBY_BUILD_DIR)/host/mrblib/mrblib.c $@

BUILT_SOURCES += mrbgems_init.c
libmruby_la_SOURCES += mrbgems_init.c
mrbgems_init.c: mruby-build.timestamp
	(							\
	  cat $(MRUBY_BUILD_DIR)/host/mrbgems/gem_init.c;	\
	  cat $(MRUBY_BUILD_DIR)/host/mrbgems/*/gem_init.c;	\
	) > $@

MRUBY_CONFIG = $(abs_srcdir)/build_config.rb
mruby-build.timestamp: build_config.rb
	rm -rf $(MRUBY_BUILD_DIR)
	cd $(srcdir)/../mruby-source &&		\
	  $(RUBY) minirake			\
	    MRUBY_BUILD_DIR=$(MRUBY_BUILD_DIR)	\
	    MRUBY_CONFIG=$(MRUBY_CONFIG)
	touch $@

CLEANFILES = *.gcno *gcda
endif

update:
	cd "$(srcdir)/../mruby-source" && git pull --rebase
	cd "$(srcdir)" && \
	  ./update.rb build_config.rb ../mruby-source > sources.am
