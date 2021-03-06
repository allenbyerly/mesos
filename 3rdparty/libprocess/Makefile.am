# Makefile for libprocess. Note that 3rdparty needs to be built
# first (see 3rdparty/Makefile.am).

ACLOCAL_AMFLAGS = -I m4

AUTOMAKE_OPTIONS = foreign

SUBDIRS = 3rdparty .

include 3rdparty/versions.am

STOUT = 3rdparty/stout
BOOST = 3rdparty/boost-$(BOOST_VERSION)
GLOG = 3rdparty/glog-$(GLOG_VERSION)
GMOCK = 3rdparty/gmock-$(GMOCK_VERSION)
GPERFTOOLS = 3rdparty/gperftools-$(GPERFTOOLS_VERSION)
GTEST = $(GMOCK)/gtest
LIBEV = 3rdparty/libev-$(LIBEV_VERSION)
PICOJSON = 3rdparty/picojson-$(PICOJSON_VERSION)


# Library. It is not installable presently because most people link
# the libprocess statically into their resulting library or binary and
# don't want any parts of libprocess to get installed (which happens
# even if you attempt to do conditional installation via configure
# arguments, see configure.ac).
noinst_LTLIBRARIES = libprocess.la

libprocess_la_SOURCES =		\
  src/config.hpp		\
  src/decoder.hpp		\
  src/encoder.hpp		\
  src/gate.hpp			\
  src/http.cpp			\
  src/latch.cpp			\
  src/metrics/metrics.cpp	\
  src/pid.cpp			\
  src/process.cpp		\
  src/reap.cpp			\
  src/subprocess.cpp		\
  src/synchronized.hpp		\
  src/timeseries.cpp

libprocess_la_CPPFLAGS =		\
  -I$(srcdir)/include			\
  -I$(srcdir)/$(STOUT)/include		\
  -I$(BOOST)				\
  -I$(GLOG)/src				\
  -I$(LIBEV)				\
  -I$(PICOJSON)				\
  $(AM_CPPFLAGS)

if WITH_BUNDLED_HTTP_PARSER
  RY_HTTP_PARSER = 3rdparty/ry-http-parser-$(RY_HTTP_PARSER_VERSION)
  libprocess_la_CPPFLAGS += -I$(RY_HTTP_PARSER)

  HTTP_PARSER_LIB = 3rdparty/libry_http_parser.la
else
  HTTP_PARSER_LIB = -lhttp_parser
endif

libprocess_la_LIBADD =			\
  $(GLOG)/libglog.la			\
  $(HTTP_PARSER_LIB)			\
  $(LIBEV)/libev.la

if HAS_GPERFTOOLS
libprocess_la_CPPFLAGS += -I$(GPERFTOOLS)/src
libprocess_la_LIBADD += $(GPERFTOOLS)/libprofiler.la
endif

# Headers.
libprocess_la_SOURCES +=					\
  $(top_srcdir)/include/process/async.hpp			\
  $(top_srcdir)/include/process/check.hpp			\
  $(top_srcdir)/include/process/clock.hpp			\
  $(top_srcdir)/include/process/collect.hpp			\
  $(top_srcdir)/include/process/defer.hpp			\
  $(top_srcdir)/include/process/deferred.hpp			\
  $(top_srcdir)/include/process/delay.hpp			\
  $(top_srcdir)/include/process/dispatch.hpp			\
  $(top_srcdir)/include/process/event.hpp			\
  $(top_srcdir)/include/process/executor.hpp			\
  $(top_srcdir)/include/process/filter.hpp			\
  $(top_srcdir)/include/process/future.hpp			\
  $(top_srcdir)/include/process/gc.hpp				\
  $(top_srcdir)/include/process/gmock.hpp			\
  $(top_srcdir)/include/process/gtest.hpp			\
  $(top_srcdir)/include/process/help.hpp			\
  $(top_srcdir)/include/process/http.hpp			\
  $(top_srcdir)/include/process/id.hpp				\
  $(top_srcdir)/include/process/internal.hpp			\
  $(top_srcdir)/include/process/io.hpp				\
  $(top_srcdir)/include/process/latch.hpp			\
  $(top_srcdir)/include/process/limiter.hpp			\
  $(top_srcdir)/include/process/logging.hpp			\
  $(top_srcdir)/include/process/message.hpp			\
  $(top_srcdir)/include/process/metrics/counter.hpp		\
  $(top_srcdir)/include/process/metrics/gauge.hpp		\
  $(top_srcdir)/include/process/metrics/metric.hpp		\
  $(top_srcdir)/include/process/metrics/metrics.hpp		\
  $(top_srcdir)/include/process/metrics/timer.hpp		\
  $(top_srcdir)/include/process/mime.hpp			\
  $(top_srcdir)/include/process/mutex.hpp			\
  $(top_srcdir)/include/process/once.hpp			\
  $(top_srcdir)/include/process/owned.hpp			\
  $(top_srcdir)/include/process/pid.hpp				\
  $(top_srcdir)/include/process/process.hpp			\
  $(top_srcdir)/include/process/profiler.hpp			\
  $(top_srcdir)/include/process/protobuf.hpp			\
  $(top_srcdir)/include/process/queue.hpp			\
  $(top_srcdir)/include/process/reap.hpp			\
  $(top_srcdir)/include/process/run.hpp				\
  $(top_srcdir)/include/process/sequence.hpp			\
  $(top_srcdir)/include/process/shared.hpp			\
  $(top_srcdir)/include/process/socket.hpp			\
  $(top_srcdir)/include/process/statistics.hpp			\
  $(top_srcdir)/include/process/system.hpp			\
  $(top_srcdir)/include/process/subprocess.hpp			\
  $(top_srcdir)/include/process/time.hpp			\
  $(top_srcdir)/include/process/timeout.hpp			\
  $(top_srcdir)/include/process/timer.hpp			\
  $(top_srcdir)/include/process/timeseries.hpp

# C++11 Headers.
libprocess_la_SOURCES +=					\
  $(top_srcdir)/include/process/c++11/defer.hpp			\
  $(top_srcdir)/include/process/c++11/deferred.hpp		\
  $(top_srcdir)/include/process/c++11/delay.hpp			\
  $(top_srcdir)/include/process/c++11/dispatch.hpp		\
  $(top_srcdir)/include/process/c++11/executor.hpp

# Tests.
check_PROGRAMS = tests

tests_SOURCES =							\
  src/tests/decoder_tests.cpp					\
  src/tests/encoder_tests.cpp					\
  src/tests/http_tests.cpp					\
  src/tests/io_tests.cpp					\
  src/tests/main.cpp						\
  src/tests/mutex_tests.cpp					\
  src/tests/metrics_tests.cpp					\
  src/tests/owned_tests.cpp					\
  src/tests/process_tests.cpp					\
  src/tests/queue_tests.cpp					\
  src/tests/reap_tests.cpp					\
  src/tests/sequence_tests.cpp					\
  src/tests/shared_tests.cpp					\
  src/tests/statistics_tests.cpp				\
  src/tests/subprocess_tests.cpp				\
  src/tests/system_tests.cpp					\
  src/tests/timeseries_tests.cpp				\
  src/tests/time_tests.cpp

tests_CPPFLAGS =			\
  -I$(top_srcdir)/src			\
  -I$(GTEST)/include			\
  -I$(GMOCK)/include			\
  $(libprocess_la_CPPFLAGS)

tests_LDADD =				\
  3rdparty/libgmock.la			\
  libprocess.la				\
  $(GLOG)/libglog.la			\
  $(HTTP_PARSER_LIB)			\
  $(LIBEV)/libev.la

# We use a check-local target for now to avoid the parallel test
# runner that ships with newer versions of autotools.
# See the following discussion for the workaround:
# http://lists.gnu.org/archive/html/automake/2013-01/msg00051.html
check-local: tests
	./tests

# TODO(benh): Fix shared builds (tests need libglog, libev, etc).

# Using LT_OUTPUT in configure.ac creates config.lt that doesn't get
# cleaned up by distclean-libtool. See this bug patch (which doesn't
# appear to be in all versions of libtool.m4):
# http://lists.gnu.org/archive/html/automake-commit/2008-11/msg00015.html.
distclean-local:
	-rm -f config.lt
