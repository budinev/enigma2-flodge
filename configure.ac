AC_INIT([enigma2],[4],[forum@openpli.org])
AM_INIT_AUTOMAKE([dist-bzip2 no-define tar-pax -Wno-portability])
AC_CONFIG_HEADERS([enigma2_config.h])

# Silent rules are available since 1.11, but older versions
# are still in use. So don't use them unconditionally.
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

AC_USE_SYSTEM_EXTENSIONS
AC_SYS_LARGEFILE

AC_PROG_CC
AC_PROG_CXX
m4_ifdef([LT_INIT], [LT_INIT], [AC_PROG_LIBTOOL])
AC_CONFIG_MACRO_DIR([m4])
AC_OPENMP()

AC_PATH_PROG(MSGFMT, msgfmt, AC_MSG_ERROR(Could not find msgfmt))

AM_PATH_PYTHON
AX_PYTHON_DEVEL
AX_PKG_SWIG
AX_SWIG_ENABLE_CXX
AX_SWIG_PYTHON
AX_PTHREAD

TUXBOX_APPS_DVB

AM_CONDITIONAL(HAVE_GIT_DIR, test -d "$srcdir/.git")
AM_CONDITIONAL(HAVE_FAKE_GIT_DIR, test -f "$srcdir/.git/last_commit_info")

dnl versions of plugins-base
PKG_CHECK_MODULES(TUXTXT, tuxbox-tuxtxt)
PKG_CHECK_MODULES(BASE, [freetype2 fribidi glib-2.0 libdvbsi++ libpng libxml-2.0 sigc++-3.0 libssl libcrypto])
PKG_CHECK_MODULES(LIBDDVD, libdreamdvd, HAVE_LIBDDVD="yes", HAVE_LIBDDVD="no")
AM_CONDITIONAL(HAVE_LIBDDVD, test "$HAVE_LIBDDVD" = "yes")
PKG_CHECK_MODULES(AVAHI, avahi-client)

AC_CHECK_LIB([udfread], [udfread_init])
AC_CHECK_LIB([dl], [dlopen], [LIBDL_LIBS="-ldl"], [AC_MSG_ERROR([Could not find libdl])])
AC_SUBST(LIBDL_LIBS)
AC_CHECK_LIB([jpeg], [jpeg_set_defaults], [LIBJPEG_LIBS="-ljpeg"], [AC_MSG_ERROR([Could not find libjpeg])])
AC_SUBST(LIBJPEG_LIBS)
AC_CHECK_LIB([ungif], [DGifOpen], [LIBGIF_LIBS="-lungif"], [AC_CHECK_LIB([gif], [DGifOpen], [LIBGIF_LIBS="-lgif"], [AC_MSG_ERROR([Could not find libgif or libungif])])])
AC_SUBST(LIBGIF_LIBS)

AC_ARG_WITH(crashlogemail,
	[  --with-crashlogemail=crash log email address],
	[CRASHLOGEMAIL="$withval"],[CRASHLOGEMAIL="the OpenPLi forum"])
AC_SUBST(CRASHLOGEMAIL)
AC_DEFINE_UNQUOTED(CRASH_EMAILADDR,"$CRASHLOGEMAIL",[crash log email address])

AC_ARG_WITH(boxtype,
	[  --with-boxtype=NAME box type [[none,dm800...]]],
	[BOXTYPE="$withval"],[BOXTYPE="dm800"])
AC_SUBST(BOXTYPE)
AC_DEFINE_UNQUOTED(BOXTYPE,"$BOXTYPE",[box type])

AM_CONDITIONAL(DREAMONE, test "$BOXTYPE" == dreamone)
AM_CONDITIONAL(DREAMTWO, test "$BOXTYPE" == dreamtwo)
AM_CONDITIONAL(DM900, test "$BOXTYPE" == dm900)
AM_CONDITIONAL(DM920, test "$BOXTYPE" == dm920)
AM_CONDITIONAL(DM8000, test "$BOXTYPE" == dm8000)
AM_CONDITIONAL(DM7080, test "$BOXTYPE" == dm7080)
AM_CONDITIONAL(DM820, test "$BOXTYPE" == dm820)
AM_CONDITIONAL(DM520, test "$BOXTYPE" == dm520)
AM_CONDITIONAL(DM7020HD, test "$BOXTYPE" == dm7020hd)
AM_CONDITIONAL(DM7020HDV2, test "$BOXTYPE" == dm7020hdv2)
AM_CONDITIONAL(DM800, test "$BOXTYPE" == dm800)
AM_CONDITIONAL(DM800SE, test "$BOXTYPE" == dm800se)
AM_CONDITIONAL(DM800SEV2, test "$BOXTYPE" == dm800sev2)
AM_CONDITIONAL(DM500HD, test "$BOXTYPE" == dm500hd)
AM_CONDITIONAL(DM500HDV2, test "$BOXTYPE" == dm500hdv2)

AM_CONDITIONAL(HAVE_COLORLCD3, test "$BOXTYPE" == dm7080)
AM_CONDITIONAL(HAVE_COLORLCD2, test "$BOXTYPE" == dm800se -o "$BOXTYPE" == dm800sev2 -o "$BOXTYPE" == dm820)
AM_CONDITIONAL(HAVE_COLORLCD1, test "$BOXTYPE" == dm8000 -o "$BOXTYPE" == dm800 -o "$BOXTYPE" == dm7020hd -o "$BOXTYPE" == dm7020hdv2)
AM_CONDITIONAL(HAVE_GRAUTEC8000, test "$BOXTYPE" == dm8000)
AM_CONDITIONAL(HAVE_GRAUTEC70X0, test "$BOXTYPE" == dm7020hd -o "$BOXTYPE" == dm7020hdv2 -o "$BOXTYPE" == dm7080)

AM_CONDITIONAL(DMAMLOGIC, test "$BOXTYPE" == "dreamone" -o "$BOXTYPE" == "dreamtwo")
AM_CONDITIONAL(DREAMBOX, test `echo "$BOXTYPE" | cut -b 1-2` == "dm")
AM_CONDITIONAL(HAVE_TEMPFANCONTROL, test "$BOXTYPE" == dm8000 -o "$BOXTYPE" == dm800se -o "$BOXTYPE" == dm500hd)
if test `echo "$BOXTYPE" | cut -b 1-2` == "dm"; then
	AC_DEFINE(DREAMBOX, 1, [Dreambox hardware])
	AC_DEFINE(KEY_PLAY_ACTUALLY_IS_KEY_PLAYPAUSE, 1,[define when rc sends a KEY_PLAY event for its KEY_PLAYPAUSE key])
fi

if test "$BOXTYPE" == "dreamone" -o "$BOXTYPE" == "dreamtwo"; then
	AC_DEFINE(DMAMLOGIC, 1, [Dreambox Amlogic hardware])
fi

if test "$BOXTYPE" == "dm7080" -o "$BOXTYPE" == "dm820" -o "$BOXTYPE" == "dm800se" -o "$BOXTYPE" == "dm800sev2" -o "$BOXTYPE" == "dm7020hd" -o "$BOXTYPE" == "dm7020hdv2" -o "$BOXTYPE" == "dm900" -o "$BOXTYPE" == "dm920" -o "$BOXTYPE" == "dreamone" -o "$BOXTYPE" == "dreamtwo"; then
	AC_DEFINE(DREAMBOX_DUAL_TUNER, 1,[define dreambox dual tuner])
fi

if test "$BOXTYPE" == "dm7080" -o "$BOXTYPE" == "dm520" -o "$BOXTYPE" == "dm820"  -o "$BOXTYPE" == "dm900" -o "$BOXTYPE" == "dm920" -o "$BOXTYPE" == "dreamone" -o "$BOXTYPE" == "dreamtwo"; then
	AC_DEFINE(CONFIG_ION, 1,[define BOX use ION Allocator])
fi

if test "$BOXTYPE" == "dm900" -o "$BOXTYPE" == "dm920"; then
	AC_DEFINE(DREAMBOX_MOVE_LCD, 1,[define when we need to move lcd text a bit to the right])
fi

if test "$BOXTYPE" == "dreamone" -o "$BOXTYPE" == "dreamtwo"; then
	AC_DEFINE(HAVE_AMLOGIC, 1,[define when we need to use amlogic])
fi

AC_ARG_WITH(nolcd,
	AC_HELP_STRING([--with-nolcd], [disable ascii based lcd, yes or no]),
	[[withnolcd=$withval]],
	[[withnolcd=no]]
)
if test x"$withnolcd" != xno ; then
	AC_DEFINE(NO_LCD, 1,[Define when disable an ascii based lcd device])
fi
AM_CONDITIONAL(NO_LCD, test x"$withnolcd" != xno)

AC_ARG_WITH(textlcd,
	AC_HELP_STRING([--with-textlcd], [use ascii based lcd, yes or no]),
	[[withtextlcd=$withval]],
	[[withtextlcd=no]]
)
if test x"$withtextlcd" != xno ; then
	AC_DEFINE(HAVE_TEXTLCD, 1,[Define when using an ascii based lcd device])
fi
AM_CONDITIONAL(HAVE_TEXTLCD, test x"$withtextlcd" != xno)

AC_ARG_WITH(7segment,
	AC_HELP_STRING([--with-7segment], [use 7 segment lcd, yes or no]),
	[[TEXTSKIN="7segment"]],
	[[TEXTSKIN="default"]]
)
AC_SUBST(TEXTSKIN)

if test `echo "$BOXTYPE" | cut -b 1-7` == "mbmicro"; then
	AC_DEFINE(FORCE_NO_BLENDING_ACCELERATION, 1,[define when the framebuffer acceleration does not have alphablending support, though the autodetection might indicate that it does])
	AC_DEFINE(FORCE_NO_FILL_ACCELERATION, 1,[define when the framebuffer acceleration does not have fill support])
	AC_DEFINE(FORCE_ADVANCED_REMOTE, 1,[define to fixup the input device identification when the remote control is actually an 'advanced' remote (with play/forward/rewind keys)])
	AC_DEFINE(KEY_PLAY_ACTUALLY_IS_KEY_PLAYPAUSE, 1,[define when rc sends a KEY_PLAY event for its KEY_PLAYPAUSE key])
fi

if test `echo "$BOXTYPE" | cut -b 1-2` == "vu"; then
	AC_DEFINE(FORCE_NO_BLENDING_ACCELERATION, 1,[define when the framebuffer acceleration does not have alphablending support, though the autodetection might indicate that it does])
	AC_DEFINE(FORCE_NO_FILL_ACCELERATION, 1,[define when the framebuffer acceleration does not have fill support])
	AC_DEFINE(FORCE_ADVANCED_REMOTE, 1,[define to fixup the input device identification when the remote control is actually an 'advanced' remote (with play/forward/rewind keys)])
fi

AC_ARG_WITH(libsdl,
	AS_HELP_STRING([--with-libsdl],[use libsdl, yes or no]),
	[with_libsdl=$withval],[with_libsdl=no])
if test "$with_libsdl" = "yes"; then
	PKG_CHECK_MODULES(LIBSDL, sdl)
fi
AM_CONDITIONAL(HAVE_LIBSDL, test "$with_libsdl" = "yes")

AC_ARG_WITH(libxine,
	AS_HELP_STRING([--with-libxine],[use libxine, yes or no]),
	[with_libxine=$withval],[with_libxine=no])
if test "$with_libxine" = "yes"; then
	PKG_CHECK_MODULES(LIBXINE, libxine)
	AC_DEFINE([WITH_XINE],[1],[Define to 1 if you have libxine])
fi
AM_CONDITIONAL(HAVE_LIBXINE, test "$with_libxine" = "yes")

AC_ARG_WITH(debug,
	AS_HELP_STRING([--without-debug],[disable debugging code]),
	[with_debug="$withval"],[with_debug="yes"])
if test "$with_debug" = "yes"; then
	AC_DEFINE([DEBUG],[1],[Define to 1 to enable debugging code])
fi

AC_ARG_WITH(memcheck,
	AS_HELP_STRING([--with-memcheck],[enable memory leak checks]),
	[with_memcheck="$withval"],[with_memcheck="no"])
if test "$with_memcheck" = "yes"; then
	AC_DEFINE([MEMLEAK_CHECK],[1],[Define to 1 to enable memory leak checks])
fi

AC_ARG_WITH(po,
	AS_HELP_STRING([--with-po],[enable updating of po files]),
	[with_po="$withval"],[with_po="no"])
if test "$with_po" = "yes"; then
	AC_PATH_PROG(MSGINIT, msginit)
	AC_PATH_PROG(MSGMERGE, msgmerge)
	AC_PATH_PROG(MSGUNIQ, msguniq)
	AC_PATH_PROG(MSGATTRIB, msgattrib)
	AC_PATH_PROG(XGETTEXT, xgettext)
	if test -z "$MSGINIT" -o -z "$MSGMERGE" -o -z "$MSGUNIQ" -o -z "$MSGATTRIB" -o -z "$XGETTEXT"; then
		AC_MSG_ERROR([Could not find required gettext tools])
	fi
fi
AM_CONDITIONAL(UPDATE_PO, test "$with_po" = "yes")

AC_ARG_WITH(alsa,
	AC_HELP_STRING([--with-alsa], [Enable ALSA support]),
	[[with_alse=$withval]],
	[[with_alse=no]]
)
if test x"$with_alsa" == xyes ; then
	AC_DEFINE(HAVE_ALSA, 1,[Enables ALSA support])
	PKG_CHECK_MODULES(ALSA, alsa)
fi

AC_ARG_WITH(alsacard,
	AC_HELP_STRING([--with-alsacard=card], [use 'card' for alsa card device, e.g. hw:0]),
	[[with_alsacard=$withval]],
	[[with_alsacard=no]]
)
if test x"$with_alsacard" != xno ; then
	AC_DEFINE_UNQUOTED(ALSA_CARD, "$withval",[Defines which alsa card to use])
fi

AC_ARG_WITH(alsamixer,
	AC_HELP_STRING([--with-alsamixer=mixername], [use alsa 'mixername' for volume control, e.g. Master]),
	[[with_alsamixer=$withval]],
	[[with_alsamixer=no]]
)
if test x"$with_alsamixer" != xno ; then
	AC_DEFINE_UNQUOTED(ALSA_VOLUME_MIXER, "$withval",[Defines which alsa mixer to use for volume control])
fi

AC_ARG_WITH(fbdev,
	AC_HELP_STRING([--with-fbdev=devicename], [use devicename for framebuffer device, e.g. /dev/fb0 ]),
	[[with_fbdev=$withval]],
	[[with_fbdev=no]]
)
if test x"$with_fbdev" != xno ; then
	AC_DEFINE_UNQUOTED(FB_DEV, "$withval", [Defines which device to use for main framebuffer (e.g. /dev/fb0)])
fi

AC_ARG_WITH(lcddev,
	AC_HELP_STRING([--with-lcddev=devicename], [use devicename for lcd framebuffer device, e.g. /dev/fb1 ]),
	[[with_lcddev=$withval]],
	[[with_lcddev=no]]
)
if test x"$with_lcddev" != xno ; then
	AC_DEFINE_UNQUOTED(LCD_DEV, "$withval", [Defines which device to use for lcd (e.g. /dev/fb1)])
fi

AC_ARG_WITH(alphablendingacceleration,
	AC_HELP_STRING([--with-alphablendingacceleration=never/always/auto], [alphablending acceleration strategy: never accelerate / always accelerate / automatically decide when to accelerate (default)]),
	[[with_alphablendingacceleration=$withval]],
	[[with_alphablendingacceleration=auto]]
)

if test x"$with_alphablendingacceleration" = xnever ; then
	AC_DEFINE_UNQUOTED(FORCE_NO_BLENDING_ACCELERATION, 1, [Do not attempt to accelerate alphablending blits])
elif test x"$with_alphablendingacceleration" = xalways ; then
	AC_DEFINE_UNQUOTED(FORCE_ALPHABLENDING_ACCELERATION, 1, [Accelerate alphablending blits])
fi

AC_ARG_WITH(accelerationthreshold,
	AC_HELP_STRING([--with-accelerationthreshold=n], [use acceleration threshold of n bytes]),
	[[with_accelerationthreshold=$withval]],
	[[with_accelerationthreshold=48000]]
)

AC_DEFINE_UNQUOTED(GFX_SURFACE_ACCELERATION_THRESHOLD, $with_accelerationthreshold,[Configure threshold for accelerated surfaces (in bytes)])

AC_ARG_WITH(fillaccelerationthreshold,
	AC_HELP_STRING([--with-fillaccelerationthreshold=n], [use fill acceleration threshold of n bytes]),
	[[with_fillaccelerationthreshold=$withval]],
	[[with_fillaccelerationthreshold=80000]]
)

AC_DEFINE_UNQUOTED(GFX_SURFACE_FILL_ACCELERATION_THRESHOLD, $with_fillaccelerationthreshold,[Configure threshold for accelerated fill operations (in bytes)])

AC_ARG_WITH(blitaccelerationthreshold,
	AC_HELP_STRING([--with-blitaccelerationthreshold=n], [use blit acceleration threshold of n bytes]),
	[[with_blitaccelerationthreshold=$withval]],
	[[with_blitaccelerationthreshold=0]]
)

AC_DEFINE_UNQUOTED(GFX_SURFACE_BLIT_ACCELERATION_THRESHOLD, $with_blitaccelerationthreshold,[Configure threshold for accelerated blit operations (in bytes)])

AC_ARG_WITH(accelerationaccumulation,
	AC_HELP_STRING([--with-accelerationaccumulation], [accumulate blit/fill acceleration operations]),
	[[with_accelerationaccumulation=$withval]],
	[[with_accelerationaccumulation=no]]
)

if test x"$with_accelerationaccumulation" = xyes ; then
	AC_DEFINE(SUPPORT_ACCUMULATED_ACCELERATION_OPERATIONS, 1,[Accumulate blit/fill acceleration operations])
fi

AC_ARG_WITH(olde2api,
	AS_HELP_STRING([--with-olde2api],[use olde2api, yes or no]),
	[with_olde2api=$withval],[with_olde2api=no])
if test x"$with_olde2api" != xno ; then
	AC_DEFINE(HAVE_OLDE2_API, 1,[define add old api of enigma2])
fi

AC_CHECK_HEADERS([sys/klog.h])
AC_CHECK_FUNCS([klogctl])

AC_CHECK_HEADERS([shadow.h])
AC_CHECK_LIB([crypt], [crypt], [BASE_LIBS+=" -lcrypt"], [AC_MSG_ERROR([Could not find crypt])])
AC_CHECK_LIB([rt], [clock_gettime], [BASE_LIBS+=" -lrt"], [AC_MSG_ERROR([Could not find rt])])

ENIGMA2_CFLAGS="-fno-rtti -fno-exceptions -rdynamic -funwind-tables"
AC_SUBST(ENIGMA2_CFLAGS)

AX_CXX_COMPILE_STDCXX_17([ext],[mandatory])

AX_CHECK_COMPILE_FLAG([-Wattribute-alias], [CXXFLAGS="$CXXFLAGS -Wattribute-alias"])
AX_CHECK_COMPILE_FLAG([-Wattribute-alias=1], [CXXFLAGS="$CXXFLAGS -Wattribute-alias=1"])

CPPFLAGS="$CPPFLAGS $PYTHON_CPPFLAGS"
CFLAGS="$CFLAGS -Wall"
CXXFLAGS="$CXXFLAGS -Wall -Wextra -Werror \
	-Wformat=2 -Wformat-overflow=2 -Wformat-nonliteral -Wformat-security \
	-Wimplicit-fallthrough=5 -Wduplicated-branches -Wtrampolines \
	-Wdangling-else -Wshift-overflow=2 -Wswitch \
	-Wunused-but-set-parameter -Wunused-const-variable=1 -Wuninitialized \
	-Wstringop-overflow=3 -Woverlength-strings -Wno-error=deprecated-declarations\
	-Wunsafe-loop-optimizations -Wpointer-arith \
	-Werror=use-after-free \
	-Wno-error=unused-result -Wno-error=dangling-pointer= \
	-Wno-error=mismatched-new-delete -Wfloat-equal -Wlogical-op \
	-Wno-error=cast-align=strict -Wno-error=ignored-qualifiers \
	-Wno-error=cast-function-type -Wno-error=deprecated-declarations \
	-Wno-error=stringop-truncation -Wno-error=shadow -Wno-error=cast-qual \
	-Wno-error=aggregate-return -Wno-error=missing-field-initializers \
	-Wno-error=packed -Wno-error=vla -Wno-error=clobbered -Wno-error=unused-parameter \
	$BASE_CFLAGS $ENIGMA2_CFLAGS $PTHREAD_CFLAGS $OPENMP_CFLAGS $ALSA_CFLAGS"

ARFLAGS="cr"
AC_SUBST(ARFLAGS)
AR_FLAGS="cr"
AC_SUBST(AR_FLAGS)

AC_CONFIG_FILES([
Makefile
data/Makefile
data/fonts/Makefile
data/countries/Makefile
data/display/Makefile
data/extensions/Makefile
data/keymaps/Makefile
data/hw_info/Makefile
data/rc_models/Makefile
data/skin_default/Makefile
data/skin_default/menu/Makefile
data/skin_default/icons/Makefile
data/skin_default/buttons/Makefile
data/skin_default/epg/Makefile
data/skin_default/spinner/Makefile
data/skin_default/LCDIcons/Makefile
data/VirtualKeyBoard_Icons/Makefile
data/VirtualKeyBoard_Icons/buttonsHD/Makefile
data/VirtualKeyBoard_Icons/buttonsFHD/Makefile
include/Makefile
lib/Makefile
lib/base/eenv.cpp
lib/python/Makefile
lib/python/Components/Makefile
lib/python/Components/Converter/Makefile
lib/python/Components/Renderer/Makefile
lib/python/Components/Sources/Makefile
lib/python/Screens/Makefile
lib/python/Plugins/Makefile
lib/python/Plugins/SystemPlugins/CableScan/Makefile
lib/python/Plugins/SystemPlugins/FastScan/Makefile
lib/python/Plugins/SystemPlugins/OSDPositionSetup/Makefile
lib/python/Plugins/SystemPlugins/OSD3DSetup/Makefile
lib/python/Plugins/SystemPlugins/HdmiCEC/Makefile
lib/python/Plugins/SystemPlugins/VideoClippingSetup/Makefile
lib/python/Plugins/Extensions/CutListEditor/Makefile
lib/python/Plugins/Extensions/CutListEditor/meta/Makefile
lib/python/Plugins/Extensions/DVDBurn/Makefile
lib/python/Plugins/Extensions/DVDBurn/meta/Makefile
lib/python/Plugins/Extensions/DVDPlayer/Makefile
lib/python/Plugins/Extensions/DVDPlayer/meta/Makefile
lib/python/Plugins/Extensions/GraphMultiEPG/Makefile
lib/python/Plugins/Extensions/GraphMultiEPG/meta/Makefile
lib/python/Plugins/Extensions/Makefile
lib/python/Plugins/Extensions/MediaPlayer/Makefile
lib/python/Plugins/Extensions/MediaPlayer/meta/Makefile
lib/python/Plugins/Extensions/MediaScanner/Makefile
lib/python/Plugins/Extensions/MediaScanner/meta/Makefile
lib/python/Plugins/Extensions/PicturePlayer/Makefile
lib/python/Plugins/Extensions/PicturePlayer/meta/Makefile
lib/python/Plugins/Extensions/PicturePlayer/data/Makefile
lib/python/Plugins/Extensions/SocketMMI/Makefile
lib/python/Plugins/Extensions/SocketMMI/meta/Makefile
lib/python/Plugins/Extensions/SocketMMI/src/Makefile
lib/python/Plugins/PLi/Makefile
lib/python/Plugins/SystemPlugins/CommonInterfaceAssignment/Makefile
lib/python/Plugins/SystemPlugins/CommonInterfaceAssignment/meta/Makefile
lib/python/Plugins/SystemPlugins/DefaultServicesScanner/Makefile
lib/python/Plugins/SystemPlugins/DefaultServicesScanner/meta/Makefile
lib/python/Plugins/SystemPlugins/DiseqcTester/Makefile
lib/python/Plugins/SystemPlugins/DiseqcTester/meta/Makefile
lib/python/Plugins/SystemPlugins/FastChannelChange/Makefile
lib/python/Plugins/SystemPlugins/FastChannelChange/meta/Makefile
lib/python/Plugins/SystemPlugins/Hotplug/Makefile
lib/python/Plugins/SystemPlugins/Hotplug/meta/Makefile
lib/python/Plugins/SystemPlugins/Makefile
lib/python/Plugins/SystemPlugins/TempFanControl/Makefile
lib/python/Plugins/SystemPlugins/TempFanControl/meta/Makefile
lib/python/Plugins/SystemPlugins/NetworkWizard/Makefile
lib/python/Plugins/SystemPlugins/NetworkWizard/meta/Makefile
lib/python/Plugins/SystemPlugins/PositionerSetup/Makefile
lib/python/Plugins/SystemPlugins/PositionerSetup/meta/Makefile
lib/python/Plugins/SystemPlugins/SatelliteEquipmentControl/Makefile
lib/python/Plugins/SystemPlugins/SatelliteEquipmentControl/meta/Makefile
lib/python/Plugins/SystemPlugins/Satfinder/Makefile
lib/python/Plugins/SystemPlugins/Satfinder/meta/Makefile
lib/python/Plugins/SystemPlugins/SoftwareManager/Makefile
lib/python/Plugins/SystemPlugins/SoftwareManager/meta/Makefile
lib/python/Plugins/SystemPlugins/VideoEnhancement/Makefile
lib/python/Plugins/SystemPlugins/VideoEnhancement/meta/Makefile
lib/python/Plugins/SystemPlugins/VideoTune/Makefile
lib/python/Plugins/SystemPlugins/VideoTune/meta/Makefile
lib/python/Plugins/SystemPlugins/Videomode/Makefile
lib/python/Plugins/SystemPlugins/Videomode/meta/Makefile
lib/python/Plugins/SystemPlugins/WirelessLan/Makefile
lib/python/Plugins/SystemPlugins/WirelessLan/meta/Makefile
lib/python/Tools/Makefile
po/Makefile
main/Makefile
tools/Makefile
tools/enigma2.sh
enigma2.pc
])
AC_OUTPUT