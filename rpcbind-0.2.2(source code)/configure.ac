AC_INIT(rpcbind, 0.2.2)
 
AM_INIT_AUTOMAKE
AC_CONFIG_SRCDIR([src/rpcbind.c])
AC_PREFIX_DEFAULT(/usr)
AC_PROG_CC

AC_ARG_ENABLE([libwrap],
  AS_HELP_STRING([--enable-libwrap], [Enables host name checking through tcpd @<:@default=no@:>@]))
AM_CONDITIONAL(LIBWRAP, test x$enable_libwrap = xyes)

AC_ARG_ENABLE([debug],
  AS_HELP_STRING([--enable-debug], [Turns on rpcbind debugging @<:@default=no@:>@]))
AM_CONDITIONAL(DEBUG, test x$enable_debug = xyes)

AC_ARG_ENABLE([warmstarts],
  AS_HELP_STRING([--enable-warmstarts], [Enables Warm Starts @<:@default=no@:>@]))
AM_CONDITIONAL(WARMSTART, test x$enable_warmstarts = xyes)

AC_ARG_WITH([statedir],
  AS_HELP_STRING([--with-statedir=ARG], [use ARG as state dir @<:@default=/tmp@:>@])
  ,, [with_statedir=/tmp])
AC_SUBST([statedir], [$with_statedir])

AC_ARG_WITH([rpcuser],
  AS_HELP_STRING([--with-rpcuser=ARG], [use ARG for RPC @<:@default=root@:>@])
  ,, [with_rpcuser=root])
AC_SUBST([rpcuser], [$with_rpcuser])
 
AC_ARG_WITH([nss_modules],
  AS_HELP_STRING([--with-nss-modules=NSS_MODULES]
  , [Sets the nss module search list to the given space-delimited string.
     For example --with-nss-modules="files altfiles" @<:@default=files@:>@])
  ,, [with_nss_modules=files])
AC_SUBST([nss_modules], [$with_nss_modules])

PKG_CHECK_MODULES([TIRPC], [libtirpc])

PKG_PROG_PKG_CONFIG
AC_ARG_WITH([systemdsystemunitdir],
  AS_HELP_STRING([--with-systemdsystemunitdir=DIR], [Directory for systemd service files]),
  [], [with_systemdsystemunitdir=$($PKG_CONFIG --variable=systemdsystemunitdir systemd)])
  if test "x$with_systemdsystemunitdir" != xno; then
    AC_SUBST([systemdsystemunitdir], [$with_systemdsystemunitdir])
     PKG_CHECK_MODULES([SYSTEMD], [libsystemd], [],
	   [PKG_CHECK_MODULES([SYSTEMD], [libsystemd-daemon], [],
	   AC_MSG_ERROR([libsystemd support requested but found]))])
  fi
AM_CONDITIONAL(SYSTEMD, [test -n "$with_systemdsystemunitdir" -a "x$with_systemdsystemunitdir" != xno ])

AS_IF([test x$enable_libwrap = xyes], [
	AC_CHECK_LIB([wrap], [hosts_access], ,
		AC_MSG_ERROR([libwrap support requested but unable to find libwrap]))
])

AC_SEARCH_LIBS([pthread_create], [pthread])

AC_OUTPUT([Makefile])
