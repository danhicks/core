AM_CFLAGS = $(ENTERPRISE_CFLAGS) \
	-I$(top_srcdir)/libpromises \
	-I$(top_srcdir)/libutils \
	-I$(top_srcdir)/libcfnet

noinst_PROGRAMS =

noinst_LTLIBRARIES = libmock-package-manager.la
libmock_package_manager_la_SOURCES = mock-package-manager.c
libmock_package_manager_la_LIBADD = ../../libpromises/libpromises.la

if !BUILTIN_EXTENSIONS
noinst_PROGRAMS += mock-package-manager

mock_package_manager_SOURCES =
mock_package_manager_LDADD = libmock-package-manager.la
endif # !BUILTIN_EXTENSIONS

if HAVE_LIBXML2
noinst_PROGRAMS += xml-c14nize

xml_c14nize_CPPFLAGS = $(LIBXML2_CPPFLAGS)
xml_c14nize_CFLAGS = -I$(top_srcdir)/libpromises -I$(srcdir)/../../libutils \
    $(LIBXML2_CFLAGS)
xml_c14nize_LDADD = ../../libutils/libutils.la \
    $(LIBXML2_LIBS)
endif

EXTRA_DIST = default.cf.sub testall

TESTS = testall
TESTS_ENVIRONMENT = env \
	AGENT=`pwd`/../../cf-agent/cf-agent \
	CF_PROMISES=`pwd`/../../cf-promises/cf-promises \
	CF_SERVERD=`pwd`/../../cf-serverd/cf-serverd \
	CF_KEY=`pwd`/../../cf-key/cf-key \
	MOCK_PACKAGE_MANAGER=`pwd`/mock-package-manager
if !HAVE_LIBXML2
TESTS_ENVIRONMENT += LIBXML2_TESTS=0
endif

dist-hook:
	cd $(srcdir); tar -c -f - [0-9]* | tar -x -C $(abspath $(distdir)) -f -
	chmod -R go-w $(distdir)

CLEANFILES = *.gcno *.gcda

pluck:
	echo '### This is an auto-generated file, see Makefile.am and `make pluck` ###' > plucked.cf.sub
	../../contrib/cf-locate/cf-locate -f -p '( if_|edit_line set_config|set_variable_values\(|edit_field|set_line_based|location|replace_with|local_cp|empty|shell|immediate|mog| m\b|recurse|tidy| all|classes_generic| start|always|^value|edit_field line|insert_lines)' ../../../masterfiles/lib/3.6 >> plucked.cf.sub
