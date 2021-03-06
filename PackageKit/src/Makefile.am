## We require new-style dependency handling.
AUTOMAKE_OPTIONS = 1.7

if HAVE_INTROSPECTION
-include $(INTROSPECTION_MAKEFILE)
INTROSPECTION_GIRS =
INTROSPECTION_SCANNER_ARGS = --add-include-path=$(srcdir) \
							 --add-include-path=$(top_builddir)/lib/packagekit-glib2
INTROSPECTION_COMPILER_ARGS = --includedir=$(srcdir) \
							  --includedir=$(top_builddir)/lib/packagekit-glib2
endif

SUBDIRS = plugins

NULL =

PK_GLIB2_LIBS =	$(top_builddir)/lib/packagekit-glib2/libpackagekit-glib2.la	\
		$(NULL)

AM_CPPFLAGS =						\
	$(GIO_CFLAGS)					\
	$(GLIB_CFLAGS)					\
	$(SQLITE_CFLAGS)				\
	$(POLKIT_CFLAGS)				\
	$(PYTHON_CFLAGS)				\
	$(NETWORK_MANAGER_CFLAGS)			\
	$(CONNMAN_CFLAGS)				\
	$(SYSTEMD_CFLAGS)				\
	$(MCE_CFLAGS)					\
	-DPACKAGE_LOCALE_DIR=\"$(localedir)\"		\
	-DBINDIR=\"$(bindir)\"				\
	-DSBINDIR=\"$(sbindir)\"			\
	-DDATADIR=\"$(datadir)\"			\
	-DPREFIX=\""$(prefix)"\"			\
	-DSYSCONFDIR=\""$(sysconfdir)"\"		\
	-DLIBDIR=\""$(libdir)"\"			\
	-DLIBEXECDIR=\""$(libexecdir)"\"		\
	-DVERSION="\"$(VERSION)\""			\
	-DPK_DATA=\"$(pkgdatadir)\"			\
	-DLOCALSTATEDIR=\""$(localstatedir)"\"		\
	-DPK_DB_DIR=\""$(PK_DB_DIR)"\"			\
	-DTESTDATADIR=\""$(top_srcdir)/data/tests"\"	\
	-DPK_COMPILATION				\
	-DPOLKIT_LOCAL_I_KNOW_API_IS_SUBJECT_TO_CHANGE	\
	-DG_LOG_DOMAIN=\"PackageKit\"			\
	-I$(top_builddir)/lib				\
	-I$(top_srcdir)/lib				\
	$(NULL)

pluginincludedir=$(includedir)/PackageKit/plugin
plugininclude_HEADERS =				\
	packagekit-plugin.h				\
	pk-shared.h					\
	pk-backend.h					\
	pk-backend-spawn.h				\
	pk-backend-job.h				\
	pk-conf.h					\
	pk-plugin.h					\
	pk-transaction.h

plugininclude_GIRSOURCES =				\
	pk-plugin.h					\
	pk-backend.c					\
	pk-backend.h					\
	pk-backend-job.c				\
	pk-backend-job.h				\
	pk-transaction.c				\
	pk-transaction.h				\
	pk-conf.c					\
	pk-conf.h					\
	pk-shared.c					\
	pk-shared.h					\
	$(NULL)

shared_SOURCES =					\
	pk-marshal.c					\
	pk-marshal.h					\
	pk-dbus.c					\
	pk-dbus.h					\
	pk-transaction.c				\
	pk-transaction.h				\
	pk-transaction-private.h			\
	pk-backend.c					\
	pk-backend.h					\
	pk-backend-job.c				\
	pk-backend-job.h				\
	pk-network.c					\
	pk-network.h					\
	pk-shared.c					\
	pk-shared.h					\
	pk-time.h					\
	pk-time.c					\
	pk-network-stack.h				\
	pk-network-stack.c				\
	pk-network-stack-unix.c				\
	pk-network-stack-unix.h				\
	pk-network-stack-nm.h				\
	pk-network-stack-connman.h			\
	pk-proc.c					\
	pk-proc.h					\
	pk-syslog.h					\
	pk-syslog.c					\
	pk-conf.c					\
	pk-conf.h					\
	pk-store.c					\
	pk-store.h					\
	pk-cache.c					\
	pk-cache.h					\
	pk-notify.c					\
	pk-notify.h					\
	pk-spawn.c					\
	pk-spawn.h					\
	pk-sysdep.h					\
	pk-sysdep.c					\
	pk-engine.h					\
	pk-engine.c					\
	pk-backend-spawn.h				\
	pk-backend-spawn.c				\
	pk-transaction-db.h				\
	pk-transaction-db.c				\
	pk-transaction-list.c				\
	pk-transaction-list.h				\
	$(NULL)

if PK_BUILD_NETWORKMANAGER
shared_SOURCES +=					\
	pk-network-stack-nm.c
endif

if PK_BUILD_CONNMAN
shared_SOURCES +=					\
	pk-network-stack-connman.c
endif

libexec_PROGRAMS =					\
	packagekitd					\
	$(NULL)

packagekitd_SOURCES =					\
	pk-main.c					\
	$(shared_SOURCES)				\
	$(NULL)

packagekitd_LDADD =					\
	$(GLIB_LIBS)					\
	$(GMODULE_LIBS)					\
	$(NETWORK_MANAGER_LIBS)				\
	$(CONNMAN_LIBS)					\
	$(SQLITE_LIBS)					\
	$(PK_GLIB2_LIBS)				\
	$(PYTHON_LIBS)					\
	$(POLKIT_LIBS)					\
	$(GIO_LIBS)					\
	$(ARCHIVE_LIBS)					\
	$(SYSTEMD_LIBS)					\
	$(MCE_LIBS)					\
	$(NULL)

packagekitd_LDFLAGS =					\
	$(PIE_LDFLAGS)					\
	$(RELRO_LDFLAGS)

packagekitd_CFLAGS =					\
	$(PIE_CFLAGS)					\
	$(WARNINGFLAGS_C)				\
	$(NULL)

if BACKEND_TYPE_BOX
packagekitd_AM_CPPFLAGS =					\
	$(BOX_CFLAGS)
packagekitd_LDADD +=					\
	$(BOX_LIBS)
endif

BUILT_SOURCES =						\
	pk-marshal.c					\
	pk-marshal.h

pk-marshal.c: pk-marshal.list
	echo "#include \"pk-marshal.h\"" > $@ && \
	glib-genmarshal $< --prefix=pk_marshal --body >> $@

pk-marshal.h: pk-marshal.list
	glib-genmarshal $< --prefix=pk_marshal --header > $@

if EGG_BUILD_TESTS

check_PROGRAMS =					\
	pk-self-test

pk_self_test_SOURCES =					\
	pk-self-test.c					\
	$(shared_SOURCES)				\
	$(NULL)

pk_self_test_LDADD =					\
	$(GLIB_LIBS)					\
	$(GMODULE_LIBS)					\
	$(SQLITE_LIBS)					\
	$(NETWORK_MANAGER_LIBS)				\
	$(CONNMAN_LIBS)					\
	$(PK_GLIB2_LIBS)				\
	$(POLKIT_LIBS)					\
	$(GIO_LIBS)					\
	$(ARCHIVE_LIBS)					\
	$(SYSTEMD_LIBS)					\
	$(MCE_LIBS)					\
	$(NULL)

pk_self_test_CFLAGS =
	-DEGG_TEST					\
	$(AM_CFLAGS)					\
	$(WARNINGFLAGS_C)				\
	$(NULL)

TESTS = pk-self-test
endif

install-data-hook:
	if test -w $(DESTDIR)$(prefix)/; then \
		mkdir -p $(DESTDIR)$(localstatedir)/cache/PackageKit/downloads; \
		mkdir -p $(DESTDIR)$(PK_LOG_DIR); \
	fi

introspectiondir = $(datadir)/dbus-1/interfaces
dist_introspection_DATA = 				\
	org.freedesktop.PackageKit.xml			\
	org.freedesktop.PackageKit.Transaction.xml

EXTRA_DIST =						\
	pk-marshal.list

clean-local:
	rm -f *~

CLEANFILES =						\
	$(BUILT_SOURCES)				\
	pk-marshal.c					\
	pk-marshal.h					\
	transactions.db

if HAVE_INTROSPECTION

# Build dummy library for g-ir-scanner to parse
noinst_LTLIBRARIES = libpkplugins.la

libpkplugins_la_SOURCES = $(plugininclude_GIRSOURCES) \
			$(shared_SOURCES)

libpkplugins_la_LIBADD =				\
	$(GLIB_LIBS)					\
	$(GMODULE_LIBS)					\
	$(NETWORK_MANAGER_LIBS)				\
	$(CONNMAN_LIBS)					\
	$(SQLITE_LIBS)					\
	$(PK_GLIB2_LIBS)				\
	$(PYTHON_LIBS)					\
	$(POLKIT_LIBS)					\
	$(GIO_LIBS)					\
	$(ARCHIVE_LIBS)					\
	$(SYSTEMD_LIBS)					\
	$(MCE_LIBS)					\
	$(NULL)

libpkplugins_la_CFLAGS =				\
	$(PIE_CFLAGS)					\
	$(WARNINGFLAGS_C)				\
	$(NULL)

PackageKitPlugin-1.0.gir: libpkplugins.la $(plugininclude_GIRSOURCES)
PackageKitPlugin_1_0_gir_INCLUDES = GObject-2.0 Gio-2.0
PackageKitPlugin_1_0_gir_CFLAGS = $(AM_CPPFLAGS)						\
				-DI_KNOW_THE_PACKAGEKIT_GLIB2_API_IS_SUBJECT_TO_CHANGE	\
				-DI_KNOW_THE_PACKAGEKIT_PLUGIN_API_IS_SUBJECT_TO_CHANGE
PackageKitPlugin_1_0_gir_SCANNERFLAGS = --identifier-prefix=Pk \
					--symbol-prefix=pk \
					--warn-all \
					--include-uninstalled=$(top_builddir)/lib/packagekit-glib2/PackageKitGlib-1.0.gir \
					--add-include-path=$(srcdir) \
					--add-include-path=$(top_builddir)/lib/packagekit-glib2 \
					--library-path=$(top_builddir)/lib/packagekit-glib2 \
					--c-include="plugin/packagekit-plugin.h"
PackageKitPlugin_1_0_gir_LIBS = libpkplugins.la
PackageKitPlugin_1_0_gir_FILES = $(plugininclude_GIRSOURCES)
INTROSPECTION_GIRS += PackageKitPlugin-1.0.gir

girdir = $(datadir)/gir-1.0
gir_DATA = $(INTROSPECTION_GIRS)

typelibdir = $(libdir)/girepository-1.0
typelib_DATA = $(INTROSPECTION_GIRS:.gir=.typelib)

CLEANFILES += $(gir_DATA) $(typelib_DATA)
endif

-include $(top_srcdir)/git.mk
