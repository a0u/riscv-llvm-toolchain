AC_INIT([riscv-llvm-toolchain], [1.0])

AC_CHECK_TOOL([CMAKE], [cmake])
AC_SUBST([CMAKE])

AC_ARG_WITH([arch],
	[AS_HELP_STRING([--with-arch=rv64imafdc],
		[Sets the base RISC-V ISA])],
	[AS_CASE(["$with_arch"],
		[*rv32* | *rv64*], [],
		[AC_MSG_ERROR([Unknown arch])])],
	[with_arch=rv64imafdc])

AC_ARG_WITH([abi],
	[AS_HELP_STRING([--with-abi=lp64d],
		[Sets the base RISC-V ABI])],
	[],
	[AS_CASE(["$with_arch"],
		[*rv64g* | *rv64*d*], [with_abi=lp64d],
		[*rv64*f*], [with_abi=lp64f],
		[*rv64*], [with_abi=lp64],
		[*rv32e*], [with_abi=ilp32e],
		[*rv32g* | *rv32*d*], [with_abi=ilp32d],
		[*rv32*f*], [with_abi=ilp32f],
		[*rv32*], [with_abi=ilp32],
		[AC_MSG_ERROR([Unknown arch])])])

AC_SUBST([arch], ["$with_arch"])
AC_SUBST([abi], ["$with_abi"])

AC_ARG_ENABLE([newlib-nano],
	[AS_HELP_STRING([--enable-newlib-nano],
		[Build reduced newlib-nano library @<:@--disable-newlib-nano@:>@])],
	[],
	[enable_newlib_nano=no])

AS_IF([test "x${enable_newlib_nano}" != xno],
	[AC_SUBST([newlib_nano], ["$enable_newlib_nano"])]
	[AC_SUBST([newlib_nano], [])])

AC_ARG_WITH([cmodel],
	[AS_HELP_STRING([--with-cmodel=medlow],
		[Select the code model to use when building libc and compiler-rt])],
	[],
	[with_cmodel=medlow])

AC_SUBST([cmodel], ["$with_cmodel"])

AC_DEFUN([AX_ARG_WITH_SRC],
	[{m4_pushdef([opt_name], $1_srcdir)
	AC_ARG_WITH([$1-src],
		[AS_HELP_STRING([--with-$1-src=DIR],
			[Set $1 source path; use builtin source by default])],
		[],
		[opt_name="\$(srcdir)/$2"])
	AC_SUBST(opt_name, ["$opt_name"])
	m4_popdef([opt_name])}])

AX_ARG_WITH_SRC([llvm], [llvm])
AX_ARG_WITH_SRC([newlib], [newlib])

AC_CONFIG_FILES([Makefile])
AC_OUTPUT
