AC_PREREQ([2.50])
AC_INIT([TheForgottenServer], [0.3.6])
AM_INIT_AUTOMAKE([1.10 foreign])
AC_CONFIG_SRCDIR([account.h])
AM_CONFIG_HEADER([config.h])

AC_PROG_CXX
AC_LANG(C++)

# check if we want server diag enabled
serverDiag=no
AC_ARG_ENABLE(server-diag, [  --enable-server-diag		enable server diagnostic],
	[OPTIONAL_FLAGS="$OPTIONAL_FLAGS -D__ENABLE_SERVER_DIAGNOSTIC__"]
	serverDiag=yes
)

# check if we want login server mode
AM_CONDITIONAL(LOGIN_SERVER, false)
loginServer=no
AC_ARG_ENABLE(login-server, [  --enable-login-server		enable login server mode],
	[OPTIONAL_FLAGS="$OPTIONAL_FLAGS -D__LOGIN_SERVER__"]
	AM_CONDITIONAL(LOGIN_SERVER, true)
	loginServer=yes
)

# check if we want remote control feature
AM_CONDITIONAL(REMOTE_CONTROL, false)
remoteControl=no
AC_ARG_ENABLE(remote-control, [  --enable-remote-control	enable remote control feature],
	[OPTIONAL_FLAGS="$OPTIONAL_FLAGS -D__REMOTE_CONTROL__"]
	AM_CONDITIONAL(REMOTE_CONTROL, true)
	remoteControl=yes
)

# check if we want otserv allocator enabled
otservAllocator=no
AC_ARG_ENABLE(otserv-allocator, [  --enable-otserv-allocator	enable otserv custom allocator],
	[OPTIONAL_FLAGS="$OPTIONAL_FLAGS -D__OTSERV_ALLOCATOR__"]
	otservAllocator=yes
)

# check if we want homedir conf enabled
homedirConf=no
AC_ARG_ENABLE(homedir-conf, [  --enable-homedir-conf		enable home directory configuration],
	[OPTIONAL_FLAGS="$OPTIONAL_FLAGS -D__HOMEDIR_CONF__"]
	homedirConf=yes
)

# check do we want to allow running on root user
rootPermission=no
AC_ARG_ENABLE(root-permission, [  --enable-root-permission	enable running on root user],
	[OPTIONAL_FLAGS="$OPTIONAL_FLAGS -D__ROOT_PERMISSION__"]
	rootPermission=yes
)
AC_SUBST(OPTIONAL_FLAGS)

# check if we want a debug build
debugBuild=no
AC_ARG_ENABLE(debug, [  --enable-debug				enable debuging],
	[DEBUG_FLAGS="-D__DEBUG__ -D__DEBUG_MOVESYS__ -D__DEBUG_CHAT__ -D__DEBUG_EXCEPTION_REPORT__ -D__DEBUG_HOUSES__ -D__DEBUG_LUASCRIPTS__ -D__DEBUG_MAILBOX__ -D__DEBUG_NET__ -D__DEBUG_NET_DETAIL__ -D__DEBUG_RAID__ -D__DEBUG_SCHEDULER__ -D__DEBUG_SPAWN__ -D__SQL_QUERY_DEBUG__"]
	debugBuild=yes
)
AC_SUBST(DEBUG_FLAGS)

AM_CONDITIONAL(USE_MYSQL, false)
useMySQL=no
AM_CONDITIONAL(USE_PGSQL, false)
usePostgreSQL=no
AM_CONDITIONAL(USE_ODBC, false)
useODBC=no
AM_CONDITIONAL(USE_SQLITE, false)
useSQLite=no

# check if we want mysql enabled
AC_ARG_ENABLE(mysql, [  --enable-mysql			enable MySQL support], [
	AM_CONDITIONAL(USE_MYSQL, true)
	MYSQL_FLAGS=-D__USE_MYSQL__
	AC_SUBST(MYSQL_FLAGS)
	useMySQL=yes
])

# check if we want postgresql enabled
AC_ARG_ENABLE(pgsql, [  --enable-pgsql			enable PostgreSQL support], [
	AM_CONDITIONAL(USE_PGSQL, true)
	PGSQL_FLAGS=-D__USE_PGSQL__
	AC_SUBST(PGSQL_FLAGS)
	usePostgreSQL=yes
])

# check if we want odbc enabled
AC_ARG_ENABLE(odbc, [  --enable-odbc			enable ODBC support], [
	AM_CONDITIONAL(USE_ODBC, true)
	ODBC_FLAGS=-D__USE_ODBC__
	AC_SUBST(ODBC_FLAGS)
	useODBC=yes
])

# check if we want sqlite enabled
AC_ARG_ENABLE(sqlite, [  --enable-sqlite		enable SQLite support], [
	AM_CONDITIONAL(USE_SQLITE, true)
	SQLITE_FLAGS=-D__USE_SQLITE__
	AC_SUBST(SQLITE_FLAGS)
	useSQLite=yes
])

#check if we want the profiler
AC_ARG_ENABLE(profiler, [  --enable-profiler		enable profiler support], [PROFILER_FLAGS=-pg])
AC_SUBST(PROFILER_FLAGS)

# Checks for header files.
AC_HEADER_STDC
AC_CHECK_HEADERS([arpa/inet.h netdb.h netinet/in.h stddef.h stdint.h stdlib.h sys/socket.h sys/timeb.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL
AC_C_CONST
AC_C_INLINE
AC_TYPE_INT16_T
AC_TYPE_INT32_T
AC_TYPE_INT64_T
AC_TYPE_SIZE_T
AC_STRUCT_TM
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT64_T
AC_TYPE_UINT8_T
AC_CHECK_TYPES([ptrdiff_t])

# Checks for library functions.
AC_FUNC_MALLOC
AC_FUNC_MEMCMP
AC_FUNC_REALLOC
AC_CHECK_FUNCS([floor ftime gethostbyname gethostname memset pow sqrt strcasecmp strncasecmp strstr strtol])

# check for libxml2
AM_PATH_XML2(2.6.5, , AC_MSG_ERROR([you need libxml2 >= 2.6.5 to compile theforgottenserver!]))
LIBS="$LIBS $XML_LIBS"

#check GMP
AC_CHECK_HEADERS([gmp.h], ,[AC_MSG_ERROR("GMP header not found.")])
AC_CHECK_LIB(gmp, __gmpz_init2, ,[AC_MSG_ERROR("Linking against GMP failed.")])

#check for boost
AC_CHECK_LIB(boost_thread-gcc-mt, main, , [
	AC_CHECK_LIB(boost_thread-mt, main, , [
		AC_CHECK_LIB(boost_thread, main, , [
			AC_MSG_ERROR("Linking against boost::thread library failed.")
		])
	])
])

AC_CHECK_LIB(boost_regex-gcc-mt, main, , [
	AC_CHECK_LIB(boost_regex-mt, main, , [
		AC_CHECK_LIB(boost_regex, main, , [
			AC_MSG_ERROR("Linking against boost::regex library failed.")
		])
	])
])

AC_CHECK_LIB(boost_system-gcc-mt, main, , [
	AC_CHECK_LIB(boost_system-mt, main, , [
		AC_CHECK_LIB(boost_system, main, , [
			AC_MSG_ERROR("Linking against boost::system library failed.")
		])
	])
])

AC_CHECK_LIB(boost_date_time-gcc-mt, main, , [
	AC_CHECK_LIB(boost_date_time-mt, main, , [
		AC_CHECK_LIB(boost_date_time, main, , [
			AC_MSG_ERROR("Linking against boost::date-time library failed.")
		])
	])
])

AC_CHECK_LIB(boost_filesystem-gcc-mt, main, , [
	AC_CHECK_LIB(boost_filesystem-mt, main, , [
		AC_CHECK_LIB(boost_filesystem, main, , [
			AC_MSG_ERROR("Linking against boost::filesystem library failed.")
		])
	])
])

# check for boost::asio
AC_CHECK_HEADERS([boost/asio.hpp], , [AC_MSG_ERROR("boost::asio header not found.")])

# check for boost::unordered_set
AC_CHECK_HEADERS([boost/unordered_set.hpp], , [AC_MSG_ERROR("boost::unordered_set header not found.")])

#check lua 5.1
PKG_CHECK_MODULES(LUA, lua5.1 >= 5.1, , [
	AC_CHECK_HEADERS([lua.hpp], , [AC_MSG_ERROR("lua header not found.")])
	AC_CHECK_LIB(lua, main, , [AC_MSG_ERROR("Linking against lua failed.")])
])
AC_SUBST(LUA_CFLAGS)
AC_SUBST(LUA_LIBS)

# check for mysql if it is enabled
if test -n "$MYSQL_FLAGS"; then
	AC_CHECK_HEADERS([mysql/mysql.h],[MYSQL_LIBS=-lmysqlclient],[AC_MSG_ERROR("mysql headers missing.")])
	AC_CHECK_LIB(mysqlclient, main,[],[AC_MSG_ERROR("Linking against mysql-client failed.")])
	AC_SUBST(MYSQL_LIBS)
fi

# check for postgresql if it is enabled
if test -n "$PGSQL_FLAGS"; then
	AC_CHECK_HEADERS([postgresql/libpq-fe.h],[PGSQL_LIBS=-lpg],[AC_MSG_ERROR("postgresql headers missing.")])
	AC_CHECK_LIB(pq, main,[],[AC_MSG_ERROR("Linking against libpq failed.")])
	AC_SUBST(PGSQL_LIBS)
fi

# check for odbc if it is enabled
if test -n "$ODBC_FLAGS"; then
	AC_CHECK_HEADERS([sql.h sqlext.h sqltypes.h],[ODBC_LIBS=-lodbc],[AC_MSG_ERROR("unixodbc headers missing.")])
	AC_CHECK_LIB(odbc, main, [], [AC_MSG_ERROR("Linking against odbc failed.")])
	AC_SUBST(ODBC_LIBS)
fi

# check for sqlite if it is enabled
if test -n "$SQLITE_FLAGS"; then
	AC_CHECK_HEADERS([sqlite3.h],[SQLITE_LIBS=-lsqlite3],[AC_MSG_ERROR("sqlite3 headers missing.")])
	AC_CHECK_LIB(sqlite3, main,[],[AC_MSG_ERROR("Linking against sqlite3 failed.")])
	AC_SUBST(SQLITE_LIBS)
fi

AC_CONFIG_FILES([Makefile])
AC_OUTPUT

echo
#echo The Forgotten Server 0.3.6
echo $PACKAGE $VERSION
echo
echo Server diagnostics.......... : $serverDiag
echo Login server mode........... : $loginServer
echo Remote control.............. : $remoteControl
echo Root run permission......... : $rootPermission
echo Home-directory configuration : $homedirConf
echo OTServ custom allocator..... : $otservAllocator
echo Debug build................. : $debugBuild
echo
echo Build with MySQL............ : $useMySQL
echo Build with SQLite........... : $useSQLite
echo Build with PostgreSQL....... : $usePostgreSQL
echo Build with ODBC............. : $useODBC
echo
if test "$useMySQL" = "no" ; then
	if test "$useSQLite" = "no" ; then
		if test "$usePostgreSQL" = "no" ; then
			if test "$useODBC" = "no" ; then
				echo Warning: no database driver was enabled! To enable a database driver, configure with --enable-\<dbdriver\> \(example: --enable-mysql\).
				echo
			fi
		fi
	fi
fi
echo Configure complete, now you may type \'make\'.
