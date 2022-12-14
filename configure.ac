AC_INIT(system-config-printer, 1.5.14)
AC_CONFIG_SRCDIR(system-config-printer.py)
AM_INIT_AUTOMAKE([dist-xz dist-bzip2 subdir-objects 1.6])
IT_PROG_INTLTOOL
AM_GNU_GETTEXT([external])
AM_PATH_PYTHON([3])
PACKAGE="AC_PACKAGE_NAME"
VERSION="AC_PACKAGE_VERSION"
GETTEXT_PACKAGE="AC_PACKAGE_NAME"
CATOBJEXT=".gmo"
DATADIRNAME=share
AC_SUBST(PACKAGE)
AC_SUBST(VERSION)
AC_SUBST(GETTEXT_PACKAGE)
AC_SUBST(CATOBJEXT)
AC_SUBST(DATADIRNAME)

# Let distributor specify if they want to use a vendor with desktop-file-install
AC_ARG_WITH(desktop-vendor,
              [AC_HELP_STRING([--with-desktop-vendor],
                              [Specify the vendor for use in calls to desktop-file-install @<:@default=@:>@])],,
                              [with_desktop_vendor=""])

VENDOR=$with_desktop_vendor
if test "x$VENDOR" = "x"; then
    DESKTOPVENDOR=
    DESKTOPPREFIX=
else
    DESKTOPVENDOR="--vendor $VENDOR"
    DESKTOPPREFIX="$VENDOR-"
fi
AC_SUBST(DESKTOPVENDOR)
AC_SUBST(DESKTOPPREFIX)

cupsserverbindir="`cups-config --serverbin`"
AC_SUBST(cupsserverbindir)

PKG_CHECK_MODULES(GLIB, glib-2.0, has_glib=yes, has_glib=no)

AC_ARG_WITH(udev-rules,
	    [AC_HELP_STRING([--with-udev-rules],
			    [Enable automatic USB print queue configuration @<:@default=no@:>@])],
	    [],
	    [with_udev_rules=no])
AM_CONDITIONAL([UDEV_RULES], [test x$with_udev_rules != xno])

AC_ARG_WITH([udevdir],
	AS_HELP_STRING([--with-udevdir=DIR], [Directory for udev helper programs]),
	[], [with_udevdir=$($PKG_CONFIG --variable=udevdir udev)])
if test "x$with_udevdir" != xno; then
	AC_SUBST([udevdir], [$with_udevdir])
	AC_SUBST([udevrulesdir], [$with_udevdir/rules.d])
fi

if test "x$with_udev_rules" != xno -a "x$with_udevdir" != xno; then
   PKG_CHECK_MODULES(libudev, [libudev >= 172], has_libudev=yes, has_libudev=no)
   PKG_CHECK_MODULES(libusb, libusb-1.0, has_libusb=yes, has_libusb=no)
   if test x$has_glib == xno -o \
           x$has_udev == xno -o \
           x$has_libudev == xno -o \
           x$has_libusb == xno ; then
      AC_MSG_ERROR([Missing packages])
   fi

   AM_PROG_CC_C_O
fi

PKG_PROG_PKG_CONFIG
AC_ARG_WITH([systemdsystemunitdir],
        AS_HELP_STRING([--with-systemdsystemunitdir=DIR], [Directory for systemd service files]),
        [], [with_systemdsystemunitdir=$($PKG_CONFIG --variable=systemdsystemunitdir systemd)])
if test "x$with_systemdsystemunitdir" != xno; then
        AC_SUBST([systemdsystemunitdir], [$with_systemdsystemunitdir])
fi
AM_CONDITIONAL(HAVE_SYSTEMD, [test -n "$with_systemdsystemunitdir" -a "x$with_systemdsystemunitdir" != xno ])

ALL_LINGUAS="ar as ast bg bn_IN bn br bs ca cs cy da de el en_GB es et fa fi fr fur gu he hi hr hu id is it ja kn ko lt lv mai ml mr ms nb nds nl nn oc or pa pl pt_BR pt ro ru si sk sl sr@latin sr sv ta te th tr uk vi zh_CN zh_TW"
AC_CONFIG_FILES([
Makefile
po/Makefile.in
system-config-printer
system-config-printer-applet
install-printerdriver
dbus/scp-dbus-service
udev/configure-printer@.service
])
AC_OUTPUT
