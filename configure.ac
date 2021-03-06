#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.61])
AC_INIT([fsharp], [3.1], [https://github.com/fsharp/fsharp/issues])

# Checks for programs.
AC_PROG_MAKE_SET

AC_PATH_PROG(PKG_CONFIG, pkg-config, no)

# On OSX use Mono's private copy of pkg-config if it exists, see https://github.com/fsharp/fsharp/issues/107
osx_pkg_config=/Library/Frameworks/Mono.framework/Versions/Current/bin/pkg-config
if test -e $osx_pkg_config; then
    PKG_CONFIG=$osx_pkg_config
elif test "x$PKG_CONFIG" = "xno"; then
        AC_MSG_ERROR([You need to install pkg-config])
fi

AC_MSG_NOTICE("pkg-config: $PKG_CONFIG")
AC_MSG_NOTICE("PKG_CONFIG_LIBDIR: $PKG_CONFIG_LIBDIR")

MONO_REQUIRED_VERSION=3.0
MONO_RECOMMENDED_VERSION=3.2

if ! $PKG_CONFIG --atleast-version=$MONO_REQUIRED_VERSION mono; then
	AC_MSG_ERROR("You need mono $MONO_REQUIRED_VERSION")
fi

if ! $PKG_CONFIG --atleast-version=$MONO_RECOMMENDED_VERSION mono; then
	AC_MSG_WARN([Mono $MONO_RECOMMENDED_VERSION or higher is recommended, for better GC performance])
fi

# Checks for libraries.

# Checks for header files.

# Checks for typedefs, structures, and compiler characteristics.

# Checks for library functions.

AC_ARG_WITH([bootstrap], 
        [  --with-bootstrap=<path>], 
        [], [with_bootstrap=lib/bootstrap])
with_bootstrap=$(cd "$with_bootstrap" && pwd)
AC_SUBST(with_bootstrap)

AC_ARG_WITH([gacdir],
                    [  --with-gacdir=/path/to/gac      Specify the gac directory (ex: /usr/lib/mono/gac)],
                [],
                [with_gacdir=no]
        )

MONOPREFIX=$(cd `$PKG_CONFIG --variable=prefix mono` && pwd)
MONOLIBDIR="$MONOPREFIX"/lib
MONOGACDIR="$MONOLIBDIR"/mono
if ! test "x$with_gacdir" = "xno"; then
	MONOGACDIR=$(cd "$with_gacdir/.." && pwd)
fi

MONOGACDIR20="$MONOGACDIR"/2.0
MONOGACDIR35="$MONOGACDIR"/3.5
MONOGACDIR40="$MONOGACDIR"/4.0

if ! test -e $MONOGACDIR20/mscorlib.dll; then
	AC_ERROR(Couldn't find the mono gac directory or mscorlib.dll in the usual places. Set --with-gacdir=<path>)
fi

if test -e $MONOLIBDIR/mono/xbuild-frameworks/.NETPortable/v4.0/Profile/Profile47/mscorlib.dll; then
   PCLENABLED=yes
else
   PCLENABLED=no
fi
AC_MSG_NOTICE(PCL Reference Assemblies found: $PCLENABLED)

AC_SUBST(PCLENABLED)
AC_SUBST(MONOLIBDIR)
AC_SUBST(MONOGACDIR)

AC_SUBST(MONOGACDIR20)
AC_SUBST(MONOGACDIR35)
AC_SUBST(MONOGACDIR40)

AC_CONFIG_FILES([
launcher
Makefile
config.make
src/fsharp/Makefile
src/fsharp/FSharp.Build-proto/Makefile
src/fsharp/FSharp.Compiler-proto/Makefile
src/fsharp/Fsc-proto/Makefile
src/fsharp/FSharp.Core/Makefile
src/fsharp/FSharp.Build/Makefile
src/fsharp/FSharp.Compiler/Makefile
src/fsharp/Fsc/Makefile
src/fsharp/FSharp.Compiler.Interactive.Settings/Makefile
src/fsharp/FSharp.Compiler.Server.Shared/Makefile
src/fsharp/FSharp.Data.TypeProviders/Makefile
src/fsharp/fsi/Makefile
src/fsharp/fsiAnyCpu/Makefile
src/fsharp/policy.2.0.FSharp.Core/Makefile
src/fsharp/policy.2.3.FSharp.Core/Makefile
src/fsharp/policy.3.3.FSharp.Core/Makefile
src/fsharp/policy.4.0.FSharp.Core/Makefile
src/fsharp/policy.4.3.FSharp.Core/Makefile
])
AC_OUTPUT

if ! test "x$MONOPREFIX" = "x$prefix"; then
	AC_WARN([Prefix to use is not the same as mono's: $prefix
		Consider using: ./autogen.sh --prefix=$MONOPREFIX])
fi
