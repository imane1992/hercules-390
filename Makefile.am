# ***************************************************************************
# Makefile for Hercules S/370, ESA/390 and z/Architecture emulator
# Process this file with 'automake' to produce Makefile.in
# ***************************************************************************

AUTOMAKE_OPTIONS = foreign 1.5

ACLOCAL_AMFLAGS = \
	-I autoconf   \
	-I m4

lns=@LN_S@

# NOte! Due to inter-module dependencies the sequence
#       (ordering) of the SUBDIRS list is important!

SUBDIRS =       \
	decNumber   \
	m4          \
	util        \
	html        \
	man         \
	.           \
	crypto

LDADD = @LIBS@

DYNAMIC_VERSION := $(shell $(top_srcdir)/_dynamic_version $(top_srcdir))

AM_CPPFLAGS =                  \
	-I$(top_srcdir)             \
	-I$(top_srcdir)/decNumber   \
	-I@HQA_INC@

if BUILD_HERCIFC
  HERCIFC = hercifc
else
  HERCIFC=
endif

if BUILD_SHARED
  HERCLIN = herclin
else
  HERCLIN =
endif

fthreads_SRC  = fthreads.c

if BUILD_FTHREADS
  FTHREADS	  = $(fthreads_SRC)
else
  FTHREADS	  =
endif

# We need to still include the source for the dynamic modules
# in with the distribution regardless of whether or not the dyanmic
# modules themselves are to be built so if dynamic load not possible,
# then we need to include the source on the EXTRA_hercules_SOURCES
# statement so even though they won't/don't actually get built, they
# still nonetheless get included into distribution tarball. This is
# *similar* to (bit not identical to) the way the fthreads sources
# are handled. With the fthreads sources though, they can at least always
# be built (even though they may not always generate any actual code
# whenever they do) whereas the dynamic module source CANNOT (MUST not)
# even be *attempted* to be "built" if support for building dynamic modules
# doesn't even exist on the system doing the building.

dynamic_SRC =   \
	dyngui.c    \
	dyninst.c   \
	hdteq.c     \
	s37x.c      \
	s37xmod.c

dyndev_SRC =    \
	awstape.c   \
	cardpch.c   \
	cardrdr.c   \
	comm3705.c  \
	commadpt.c  \
	con1052c.c  \
	console.c   \
	ctc_ctci.c  \
	ctc_lcs.c   \
	ctc_ptp.c   \
	ctcadpt.c   \
	faketape.c  \
	ftlib.c     \
	hchan.c     \
	hetlib.c    \
	hettape.c   \
	mpc.c       \
	omatape.c   \
	printer.c   \
	qeth.c      \
	resolve.c   \
	scsitape.c  \
	sllib.c     \
	sockdev.c   \
	tapeccws.c  \
	tapedev.c   \
	telnet.c    \
	tuntap.c    \
	w32ctca.c   \
	w32stape.c  \
	zfcp.c

# -module         :  create a dlopen'able module
# -no-undefined   :  required on all platform that do not allow
#                    shared modules to have undefined symbols
# $(LDADD)        :  Misc Linker flags set by autoconf
# -export-dynamic :  so dlsym works (to be verified -
#                    not sure whether it is necessary)
# -avoid-version  :  needed.. Otherwise libtool gives crazy names
#                    to Windows DLLs
#

if BUILD_SHARED
	XSTATIC =
else
	XSTATIC = -static
endif

if OPTION_DYNAMIC_LOAD

  DYNSRC  =
  LTDL    =  ltdl.c

  DYNMOD_LD_FLAGS = \
	-module         \
	-no-undefined   \
	$(XSTATIC)      \
	-export-dynamic \
	-avoid-version

  DYNMOD_LD_ADD =   \
	libherc.la      \
	libhercs.la     \
	libhercu.la     \
	$(LDADD)

  LIB_LD_FLAGS =    \
	-export-dynamic \
	 $(XSTATIC)     \
	 -no-undefined  \
	 -avoid-version

else

  DYNSRC           =  $(dyndev_SRC)
  LTDL             =
  DYNMOD_LD_FLAGS  =
  DYNMOD_LD_ADD    =

  LIB_LD_FLAGS =    \
	$(XSTATIC)      \
	-no-undefined   \
	-avoid-version

endif

#
# List of Libtool shared libraries & loadable modules
#

HERCLIBS =

HERCLIBS2 =     \
	libhercs.la \
	libhercu.la \
	libherct.la \
	libhercd.la \
	libherc.la

HERCMODS =      \
	altcmpsc.la \
	dyngui.la   \
	dyninst.la  \
	hdteq.la    \
	hdtptp.la   \
	hdtqeth.la  \
	hdtzfcp.la  \
	hdt1052c.la \
	hdt1403.la  \
	hdt2703.la  \
	hdt3705.la  \
	hdt3088.la  \
	hdt3270.la  \
	hdt3420.la  \
	hdt3505.la  \
	hdt2880.la  \
	hdt3525.la  \
	s37x.la

if OPTION_DYNAMIC_LOAD
  modexec_LTLIBRARIES = $(HERCMODS)
endif

  noinst_LTLIBRARIES = $(HERCLIBS)
  lib_LTLIBRARIES = $(HERCLIBS2)

# The following causes the dynamic modules to not even get built
# unless support for building dynamic modules exists on the build system

bin_PROGRAMS =  \
	cckdcdsk    \
	cckdcomp    \
	cckddiag    \
	cckdswap    \
	dasdcat     \
	dasdconv    \
	dasdcopy    \
	dasdinit    \
	dasdisup    \
	dasdload    \
	dasdls      \
	dasdpdsu    \
	dasdseq     \
	dmap2hrc    \
	hercules    \
	hetget      \
	hetinit     \
	hetmap      \
	hetupd      \
	tapecopy    \
	tapemap     \
	tapesplt    \
	vmfplc2     \
	$(HERCIFC)  \
	$(HERCLIN)

EXTRA_PROGRAMS = hercifc

if OPTION_DYNAMIC_LOAD

#
# For each module:
#
#	 ModuleName_la_SOURCES		 = <list of source files>
#	 ModuleName_la_LDFLAGS		 = $(DYNMOD_LD_FLAGS) (see above)
#	 ModuleName_la_LIBADD		 = libherc.la (the Core Hercules Shared Library)
#	 ModuleName_la_DEPENDENCIES = libherc.la (may not be necessary)
#
  dyngui_la_SOURCES	= dyngui.c
  dyngui_la_LDFLAGS	= $(DYNMOD_LD_FLAGS)
  dyngui_la_LIBADD	= $(DYNMOD_LD_ADD)

  dyninst_la_SOURCES = dyninst.c
  dyninst_la_LDFLAGS = $(DYNMOD_LD_FLAGS)
  dyninst_la_LIBADD	= $(DYNMOD_LD_ADD)

  s37x_la_SOURCES 	= s37x.c s37xmod.c
  s37x_la_LDFLAGS 	= $(DYNMOD_LD_FLAGS)
  s37x_la_LIBADD		= $(DYNMOD_LD_ADD)

  hdteq_la_SOURCES	= hdteq.c
  hdteq_la_LDFLAGS	= $(DYNMOD_LD_FLAGS)
  hdteq_la_LIBADD 	= $(DYNMOD_LD_ADD)

  hdt1403_la_SOURCES = printer.c sockdev.c
  hdt1403_la_LDFLAGS = $(DYNMOD_LD_FLAGS)
  hdt1403_la_LIBADD	= $(DYNMOD_LD_ADD)

  hdt2880_la_SOURCES = hchan.c
  hdt2880_la_LDFLAGS = $(DYNMOD_LD_FLAGS)
  hdt2880_la_LIBADD	= $(DYNMOD_LD_ADD)

  hdt2703_la_SOURCES = commadpt.c
  hdt2703_la_LDFLAGS = $(DYNMOD_LD_FLAGS)
  hdt2703_la_LIBADD	= $(DYNMOD_LD_ADD)

  hdt3705_la_SOURCES = comm3705.c
  hdt3705_la_LDFLAGS = $(DYNMOD_LD_FLAGS)
  hdt3705_la_LIBADD	= $(DYNMOD_LD_ADD)

  hdt3088_la_SOURCES = ctc_lcs.c ctc_ctci.c ctcadpt.c w32ctca.c tuntap.c
  hdt3088_la_LDFLAGS = $(DYNMOD_LD_FLAGS)
  hdt3088_la_LIBADD	= $(DYNMOD_LD_ADD)

  hdt3270_la_SOURCES = console.c telnet.c
  hdt3270_la_LDFLAGS = $(DYNMOD_LD_FLAGS)
  hdt3270_la_LIBADD	= $(DYNMOD_LD_ADD)

  hdt3420_la_SOURCES = tapedev.c tapeccws.c awstape.c faketape.c hettape.c omatape.c scsitape.c scsiutil.c w32stape.c
  hdt3420_la_LDFLAGS = $(DYNMOD_LD_FLAGS)
  hdt3420_la_LIBADD	= $(DYNMOD_LD_ADD) libherct.la

  hdt3505_la_SOURCES = cardrdr.c sockdev.c
  hdt3505_la_LDFLAGS = $(DYNMOD_LD_FLAGS)
  hdt3505_la_LIBADD	= $(DYNMOD_LD_ADD)

  hdt3525_la_SOURCES = cardpch.c
  hdt3525_la_LDFLAGS = $(DYNMOD_LD_FLAGS)
  hdt3525_la_LIBADD	= $(DYNMOD_LD_ADD)

  hdtqeth_la_SOURCES = qeth.c mpc.c resolve.c tuntap.c
  hdtqeth_la_LDFLAGS = $(DYNMOD_LD_FLAGS)
  hdtqeth_la_LIBADD	= $(DYNMOD_LD_ADD)

  hdtzfcp_la_SOURCES = zfcp.c
  hdtzfcp_la_LDFLAGS = $(DYNMOD_LD_FLAGS)
  hdtzfcp_la_LIBADD	= $(DYNMOD_LD_ADD)

  hdt1052c_la_SOURCES = con1052c.c
  hdt1052c_la_LDFLAGS = $(DYNMOD_LD_FLAGS)
  hdt1052c_la_LIBADD  = $(DYNMOD_LD_ADD)

  hdtptp_la_SOURCES	= ctc_ptp.c mpc.c resolve.c w32ctca.c tuntap.c
  hdtptp_la_LDFLAGS	= $(DYNMOD_LD_FLAGS)
  hdtptp_la_LIBADD	= $(DYNMOD_LD_ADD)

  altcmpsc_la_SOURCES  = cmpsc.c
  altcmpsc_la_LDFLAGS  = $(DYNMOD_LD_FLAGS)
  altcmpsc_la_LIBADD   = $(DYNMOD_LD_ADD)

endif

#
# Common data areas (shared) library
#

libhercs_la_SOURCES = hsys.c

libhercs_la_LDFLAGS = $(LIB_LD_FLAGS)

libhercs_la_LIBADD  = $(LDADD)

#
# Tape utility subroutines (shared) library
#

libherct_la_SOURCES = \
	hetlib.c          \
	ftlib.c           \
	sllib.c

libherct_la_LDFLAGS = $(LIB_LD_FLAGS)

libherct_la_LIBADD =  \
	$(LDADD)          \
	libhercs.la       \
	libhercu.la

#
# DASD utility subroutines (shared) library
#

libhercd_la_SOURCES = \
	cache.c           \
	cckddasd.c        \
	cckdutil.c        \
	ckddasd.c         \
	dasdtab.c         \
	dasdutil.c        \
	fbadasd.c         \
	shared.c

libhercd_la_LDFLAGS = $(LIB_LD_FLAGS)

libhercd_la_LIBADD =  \
	$(LDADD)          \
	libhercs.la       \
	libhercu.la

#
## Pure Utility functions
#

libhercu_la_SOURCES = \
	codepage.c        \
	hdl.c             \
	hexdumpe.c        \
	hostinfo.c        \
	hscutl.c          \
	hscutl2.c         \
	hsocket.c         \
	hthreads.c        \
	logger.c          \
	logmsg.c          \
	memrchr.c         \
	parser.c          \
	pttrace.c         \
	version.c         \
	$(FTHREADS)       \
	$(LTDL)

libhercu_la_LDFLAGS = $(LIB_LD_FLAGS)

libhercu_la_LIBADD =  \
	$(LDADD)          \
	libhercs.la

#
# Core Hercules (shared) library
#

libherc_la_SOURCES =  \
	archlvl.c         \
	assist.c          \
	bldcfg.c          \
	cgibin.c          \
	channel.c         \
	chsc.c            \
	clock.c           \
	cmdtab.c          \
	cmpsc_2012.c      \
	cmpscdbg.c        \
	cmpscdct.c        \
	cmpscget.c        \
	cmpscmem.c        \
	cmpscput.c        \
	config.c          \
	control.c         \
	cpu.c             \
	crypto.c          \
	dat.c             \
	decimal.c         \
	dfp.c             \
	diagmssf.c        \
	diagnose.c        \
	dyn76.c           \
	ecpsvm.c          \
	esame.c           \
	external.c        \
	fillfnam.c        \
	float.c           \
	general1.c        \
	general2.c        \
	general3.c        \
	hao.c             \
	hconsole.c        \
	hdiagf18.c        \
	history.c         \
	hRexx.c           \
	hRexx_o.c         \
	hRexx_r.c         \
	hsccmd.c          \
	hscemode.c        \
	hscloc.c          \
	hscmisc.c         \
	hscpufun.c        \
	httpserv.c        \
	ieee.c            \
	impl.c            \
	io.c              \
	ipl.c             \
	loadmem.c         \
	loadparm.c        \
	losc.c            \
	machchk.c         \
	opcode.c          \
	panel.c           \
	pfpo.c            \
	plo.c             \
	qdio.c            \
	scedasd.c         \
	scescsi.c         \
	script.c          \
	service.c         \
	sie.c             \
	SoftFloat-specialise.c \
	SoftFloat.c       \
	sr.c              \
	stack.c           \
	strsignal.c       \
	timer.c           \
	trace.c           \
	vector.c          \
	vm.c              \
	vmd250.c          \
	vstore.c          \
	w32util.c         \
	xstore.c          \
	$(DYNSRC)

EXTRA_libherc_la_SOURCES = \
	ltdl.c            \
	memrchr.c         \
	$(dynamic_SRC)    \
	$(dyndev_SRC)     \
	$(extra_SRC)      \
	$(fthreads_SRC)

libherc_la_LDFLAGS = $(LIB_LD_FLAGS)

libherc_la_LIBADD =   \
	libhercs.la       \
	libhercu.la       \
	libherct.la       \
	libhercd.la       \
	decNumber/libdecNumber.la \
	$(LDADD)

#
#  THIS is the hercules executable.
#  hdlmain.c is requires so that dlopen(self) retrieves
#  the 'main' hdl symbols
#

if OPTION_DYNAMIC_LOAD

if BUILD_SHARED

HLDFLAGS = -dlopen self
HDEPS =

else

HLDFLAGS =                  \
	-dlopen self            \
	-dlopen dyngui.la       \
	-dlopen dyninst.la      \
	-dlopen s37x.la         \
	-dlopen hdteq.la        \
	-dlopen hdt1403.la      \
	-dlopen hdt3420.la      \
	-dlopen hdt2703.la      \
	-dlopen hdt3705.la      \
	-dlopen hdt3088.la      \
	-dlopen hdt3270.la      \
	-dlopen hdt3505.la      \
	-dlopen hdt3525.la      \
	-dlopen hdtqeth.la      \
	-dlopen hdtzfcp.la      \
	-dlopen hdt1052c.la     \
	-dlopen hdtptp.la       \
	-dlopen altcmpsc.la

HDEPS =                     \
	dyngui.la               \
	dyninst.la              \
	s37x.la                 \
	hdteq.la                \
	hdt1403.la              \
	hdt3420.la              \
	hdt2703.la              \
	hdt3705.la              \
	hdt3088.la              \
	hdt3270.la              \
	hdt3505.la              \
	hdt3525.la              \
	hdtqeth.la              \
	hdtzfcp.la              \
	hdt1052c.la             \
	hdtptp.la               \
	altcmpsc.la             \
	libherc.la              \
	libherct.la             \
	libhercd.la

endif

else

HLDFLAGS =
HDEPS    =

endif

hercules_SOURCES =  \
	bootstrap.c     \
	hdlmain.c

hercules_LDADD =    \
	libherc.la      \
	libhercs.la     \
	$(LDADD)

hercules_LDFLAGS = $(HLDFLAGS)

hercules_DEPENDENCIES = \
	libherc.la      \
	libhercs.la     \
	$(HDEPS)

if BUILD_SHARED

herclin_SOURCES =   \
	hdlmain.c       \
	herclin.c


herclin_LDADD =     \
	libherc.la      \
	libhercs.la     \
	$(LDADD)

herclin_LDFLAGS = $(HLDFLAGS)

herclin_DEPENDENCIES =  \
	libherc.la      \
	libhercs.la     \
	$(HDEPS)

endif

#
# side binaries
#

tools_LD_FLAGS =

if OPTION_DYNAMIC_LOAD
tools_ADDLIBS = $(HERCLIBS2) $(LDADD)
else
tools_ADDLIBS = $(HERCLIBS2) $(LDADD) hdlmain.o
endif

if BUILD_HERCIFC
hercifc_SOURCES    = hercifc.c
hercifc_LDADD      = $(tools_ADDLIBS)
hercifc_LDFLAGS    = $(tools_LD_FLAGS)
endif

dasdinit_SOURCES   = dasdinit.c
dasdinit_LDADD     = $(tools_ADDLIBS)
dasdinit_LDFLAGS   = $(tools_LD_FLAGS)

dasdisup_SOURCES   = dasdisup.c
dasdisup_LDADD     = $(tools_ADDLIBS)
dasdisup_LDFLAGS   = $(tools_LD_FLAGS)

dasdload_SOURCES   = dasdload.c
dasdload_LDADD     = $(tools_ADDLIBS)
dasdload_LDFLAGS   = $(tools_LD_FLAGS)

dasdconv_SOURCES   = dasdconv.c
dasdconv_LDADD     = $(tools_ADDLIBS)
dasdconv_LDFLAGS   = $(tools_LD_FLAGS)

dasdls_SOURCES     = dasdls.c
dasdls_LDADD       = $(tools_ADDLIBS)
dasdls_LDFLAGS     = $(tools_LD_FLAGS)

dasdcat_SOURCES    = dasdcat.c
dasdcat_LDADD      = $(tools_ADDLIBS)
dasdcat_LDFLAGS    = $(tools_LD_FLAGS)

dasdpdsu_SOURCES   = dasdpdsu.c
dasdpdsu_LDADD     = $(tools_ADDLIBS)
dasdpdsu_LDFLAGS   = $(tools_LD_FLAGS)

dasdseq_SOURCES    = dasdseq.c
dasdseq_LDADD      = $(tools_ADDLIBS)
dasdseq_LDFLAGS    = $(tools_LD_FLAGS)

tapecopy_SOURCES   = tapecopy.c scsiutil.c
tapecopy_LDADD     = $(tools_ADDLIBS)
tapecopy_LDFLAGS   = $(tools_LD_FLAGS)
tapecopy_CFLAGS    = $(AM_CFLAGS)

tapemap_SOURCES    = tapemap.c
tapemap_LDADD      = $(tools_ADDLIBS)
tapemap_LDFLAGS    = $(tools_LD_FLAGS)

tapesplt_SOURCES   = tapesplt.c
tapesplt_LDADD     = $(tools_ADDLIBS)
tapesplt_LDFLAGS   = $(tools_LD_FLAGS)

cckdcdsk_SOURCES   = cckdcdsk.c
cckdcdsk_LDADD     = $(tools_ADDLIBS)
cckdcdsk_LDFLAGS   = $(tools_LD_FLAGS)

cckdcomp_SOURCES   = cckdcomp.c
cckdcomp_LDADD     = $(tools_ADDLIBS)
cckdcomp_LDFLAGS   = $(tools_LD_FLAGS)

cckddiag_SOURCES   = cckddiag.c
cckddiag_LDADD     = $(tools_ADDLIBS)
cckddiag_LDFLAGS   = $(tools_LD_FLAGS)

dasdcopy_SOURCES   = dasdcopy.c
dasdcopy_LDADD     = $(tools_ADDLIBS)
dasdcopy_LDFLAGS   = $(tools_LD_FLAGS)

cckdswap_SOURCES   = cckdswap.c
cckdswap_LDADD     = $(tools_ADDLIBS)
cckdswap_LDFLAGS   = $(tools_LD_FLAGS)

hetget_SOURCES     = hetget.c
hetget_LDADD       = $(tools_ADDLIBS)
hetget_LDFLAGS     = $(tools_LD_FLAGS)

hetinit_SOURCES    = hetinit.c
hetinit_LDADD      = $(tools_ADDLIBS)
hetinit_LDFLAGS    = $(tools_LD_FLAGS)

hetmap_SOURCES     = hetmap.c
hetmap_LDADD       = $(tools_ADDLIBS)
hetmap_LDFLAGS     = $(tools_LD_FLAGS)

hetupd_SOURCES     = hetupd.c
hetupd_LDADD       = $(tools_ADDLIBS)
hetupd_LDFLAGS     = $(tools_LD_FLAGS)

dmap2hrc_SOURCES   = dmap2hrc.c
dmap2hrc_LDADD     = $(tools_ADDLIBS)
dmap2hrc_LDFLAGS   = $(tools_LD_FLAGS)

vmfplc2_SOURCES    = vmfplc2.c
vmfplc2_LDADD      = $(tools_ADDLIBS)
vmfplc2_LDFLAGS    = $(tools_LD_FLAGS)

#
# Files that are not 'built' per-se
# Also contains some WIN32 only source files
#

EXTRA_DIST =                    \
	_build                      \
	_build.bat                  \
	_build.tail                 \
	_dynamic_version            \
	_dynamic_version.cmd        \
	_TODO.txt                   \
	all-all.jobs                \
	autoconf/config.guess       \
	autoconf/config.rpath       \
	autoconf/config.sub         \
	autoconf/depcomp            \
	autoconf/hercules.m4        \
	autoconf/install-sh         \
	autoconf/libtool.m4         \
	autoconf/ltdl.m4            \
	autoconf/ltmain.sh          \
	autoconf/missing            \
	autoconf/mkinstalldirs      \
	autoconf/README             \
	autogen.sh                  \
	bldlvlck                    \
	build_pch.c                 \
	BUILDING                    \
	CHANGES                     \
	configure.ac                \
	conspawn.c                  \
	COPYRIGHT                   \
	debug-all.jobs              \
	dynmake.bat                 \
	getopt.c                    \
	hercdasd.ico                \
	hercdasd.rc                 \
	herclogo.txt                \
	hercmisc.ico                \
	hercmisc.rc                 \
	hercprod.rc                 \
	herctape.ico                \
	herctape.rc                 \
	hercules.cnf                \
	hercules.cnf.rexx           \
	hercules.ico                \
	hercules.m4                 \
	Hercules.sln                \
	Hercules.vcproj             \
	Hercules.vcxproj            \
	Hercules.vcxproj.filters    \
	Hercules_VS2010.sln         \
	Hercules_VS2012.sln         \
	Hercules_VS2012.vcxproj     \
	hercver.rc2                 \
	INSTALL                     \
	license_dyn76.txt           \
	makefile-dllmod.msvc        \
	Makefile.am                 \
	makefile.bat                \
	makefile.msvc               \
	msvc.makefile.includes/BZIP2_DIR.msvc       \
	msvc.makefile.includes/BZIP2_FLAGS.msvc     \
	msvc.makefile.includes/BZIP2_RULES.msvc     \
	msvc.makefile.includes/CONFIG.msvc          \
	msvc.makefile.includes/DEBUG_RETAIL.msvc    \
	msvc.makefile.includes/HERC_FLAGS.msvc      \
	msvc.makefile.includes/HQA_DIR.msvc         \
	msvc.makefile.includes/HQA_FLAGS.msvc       \
	msvc.makefile.includes/IPV6_FLAGS.msvc      \
	msvc.makefile.includes/MOD_RULES1.msvc      \
	msvc.makefile.includes/MOD_RULES2.msvc      \
	msvc.makefile.includes/MODULES.msvc         \
	msvc.makefile.includes/OBJ_CODE.msvc        \
	msvc.makefile.includes/OUTDIR_RULES.msvc    \
	msvc.makefile.includes/OUTPUT_DIRS.msvc     \
	msvc.makefile.includes/PCRE_DIR.msvc        \
	msvc.makefile.includes/PCRE_FLAGS.msvc      \
	msvc.makefile.includes/PCRE_RULES.msvc      \
	msvc.makefile.includes/PRIM_RULES.msvc      \
	msvc.makefile.includes/REXX_DIRS.msvc       \
	msvc.makefile.includes/REXX_FLAGS.msvc      \
	msvc.makefile.includes/REXX_RULES.msvc      \
	msvc.makefile.includes/ZLIB_DIR.msvc        \
	msvc.makefile.includes/ZLIB_FLAGS.msvc      \
	msvc.makefile.includes/ZLIB_RULES.msvc      \
	README.AIX                  \
	README.APL360               \
	README.BSD                  \
	README.CMPSC                \
	README.COMMADPT             \
	README.DECNUMBER            \
	README.DYNMOD               \
	README.ECPSVM               \
	README.HAO                  \
	README.HDL                  \
	README.HERCLOGO             \
	README.HQA                  \
	README.HRAF                 \
	README.IOARCH               \
	README.ISSUES               \
	README.md                   \
	README.MINGW                \
	README.MSVC                 \
	README.NETWORKING           \
	README.OSX                  \
	README.PTT                  \
	README.REXX                 \
	README.RUNTEST              \
	README.RXVT4APL             \
	README.S37X                 \
	README.SETUID               \
	README.SOFTFLOAT            \
	README.SUN                  \
	README.TAPE                 \
	README.TELNET               \
	README.WIN64                \
	RELEASE.NOTES               \
	retail-all.jobs             \
	scripts/cmpsc.cnf           \
	scripts/cmpsc.rexx          \
	scripts/hbasic1.rexx        \
	scripts/hbasic2.rex         \
	scripts/hbasic3.cmd         \
	scripts/hbasic4.rx          \
	scripts/hbasic5.ext1        \
	scripts/hcommand.rexx       \
	scripts/hexecio.rexx        \
	scripts/hlinein.rexx        \
	scripts/hrecurs.rexx        \
	SoftFloat-2b.zip            \
	util/CMPSC.txt

#
# Source Header files. No 'build' for those
#

noinst_HEADERS =            \
	cache.h                 \
	ccfixme.h               \
	ccnowarn.h              \
	chain.h                 \
	chsc.h                  \
	clock.h                 \
	cmdtab.h                \
	cmpsc.h                 \
	cmpscbit.h              \
	cmpscdbg.h              \
	cmpscdct.h              \
	cmpscget.h              \
	cmpscmem.h              \
	cmpscput.h              \
	cnsllogo.h              \
	codepage.h              \
	comm3705.h              \
	commadpt.h              \
	cpuint.h                \
	ctc_ptp.h               \
	ctcadpt.h               \
	dasdblks.h              \
	dasdtab.h               \
	dat.h                   \
	dbgtrace.h              \
	devtype.h               \
	ecpsvm.h                \
	esa390.h                \
	esa390io.h              \
	extstring.h             \
	feat370.h               \
	feat390.h               \
	feat900.h               \
	featall.h               \
	featchk.h               \
	feature.h               \
	fillfnam.h              \
	fthreads.h              \
	ftlib.h                 \
	getopt.h                \
	hatomic.h               \
	hbyteswp.h              \
	hchan.h                 \
	hconsole.h              \
	hconsts.h               \
	hdiagf18.h              \
	hdl.h                   \
	herc_getopt.h           \
	hercifc.h               \
	hercules.h              \
	hercwind.h              \
	herror.h                \
	hetlib.h                \
	hexdumpe.h              \
	hextapi.h               \
	hexterns.h              \
	hifr.h                  \
	hinlines.h              \
	history.h               \
	hmacros.h               \
	hmalloc.h               \
	hostinfo.h              \
	hostopts.h              \
	hqadefs.h               \
	hqainc.h                \
	hRexx.h                 \
	hRexxapi.h              \
	hscutl.h                \
	hsocket.h               \
	hstdinc.h               \
	hstdint.h               \
	hstructs.h              \
	hthreads.h              \
	httpmisc.h              \
	htypes.h                \
	inline.h                \
	linklist.h              \
	logger.h                \
	ltdl.h                  \
	machdep.h               \
	memrchr.h               \
	mpc.h                   \
	msgenu.h                \
	opcode.h                \
	parser.h                \
	printfmt.h              \
	pttrace.h               \
	qdio.h                  \
	qeth.h                  \
	resolve.h               \
	scsitape.h              \
	scsiutil.h              \
	service.h               \
	shared.h                \
	sllib.h                 \
	sockdev.h               \
	SoftFloat-macros.h      \
	SoftFloat-milieu.h      \
	SoftFloat-specialise.h  \
	SoftFloat-types.h       \
	SoftFloat.h             \
	sr.h                    \
	tapedev.h               \
	targetver.h             \
	telnet.h                \
	tt32api.h               \
	tt32if.h                \
	tuntap.h                \
	version.h               \
	vmd250.h                \
	vstore.h                \
	w32ctca.h               \
	w32dl.h                 \
	w32mtio.h               \
	w32stape.h              \
	w32util.h               \
	zfcp.h                  \
	$(extra_dynamic_SRC)

cckd:                       \
	cckdcdsk$(EXEEXT)       \
	cckdcomp$(EXEEXT)       \
	cckddiag$(EXEEXT)       \
	cckdswap$(EXEEXT)       \
	cckd2ckd$(EXEEXT)       \
	dasdcopy$(EXEEXT)

#########################################################
#               B U I L D   R U L E S  . . .
#########################################################
#
#             ***  PROGRAMMING NOTE!  ***
#
#   The tabs in the BELOW build rules ARE significant!
#
#   (the preceding assignment statements can use blanks)
#
#   Not all make programs accept (handle correctly)
#   make files that use blanks instead of tabs in their
#   build rules. Thus in order to remain compatible
#   with older make programs the below build rules
#   MUST use *tabs* and NOT BLANKS!
#
#########################################################

tar: dist

install-exec-local:

install-exec-hook:
if BUILD_SHARED
	rm -f $(DESTDIR)$(libdir)/libherc*.a
	rm -f $(DESTDIR)$(modexecdir)/dyn*.a
	rm -f $(DESTDIR)$(modexecdir)/hdt*.a
else
	rm -f $(DESTDIR)$(libdir)/libherc*
endif
#
# NOTE : symbolic links point to FINAL destination (not to staged install)
#
	rm -f $(DESTDIR)$(bindir)/fba2cfba$(EXEEXT)
	(cd $(DESTDIR)$(bindir); @LN_S@ ./dasdcopy$(EXEEXT) fba2cfba$(EXEEXT))
	rm -f $(DESTDIR)$(bindir)/ckd2cckd$(EXEEXT)
	(cd $(DESTDIR)$(bindir); @LN_S@ ./dasdcopy$(EXEEXT) ckd2cckd$(EXEEXT))
	rm -f $(DESTDIR)$(bindir)/cfba2fba$(EXEEXT)
	(cd $(DESTDIR)$(bindir); @LN_S@ ./dasdcopy$(EXEEXT) cfba2fba$(EXEEXT))
	rm -f $(DESTDIR)$(bindir)/cckd2ckd$(EXEEXT)
	(cd $(DESTDIR)$(bindir); @LN_S@ ./dasdcopy$(EXEEXT) cckd2ckd$(EXEEXT))
if SETUID_HERCIFC
	chown root $(DESTDIR)$(bindir)/hercifc
if HERCIFC_GROUPSET
	chgrp $(HERCIFC_GROUPNAME) $(DESTDIR)$(bindir)/hercifc
endif
	chmod 0750 $(DESTDIR)$(bindir)/hercifc
	chmod +s	$(DESTDIR)$(bindir)/hercifc
	rm									  hercifc
endif

uninstall-local:

uninstall-hook:
	rm -f $(DESTDIR)$(bindir)/fba2cfba$(EXEEXT)
	rm -f $(DESTDIR)$(bindir)/ckd2cckd$(EXEEXT)
	rm -f $(DESTDIR)$(bindir)/cfba2fba$(EXEEXT)
	rm -f $(DESTDIR)$(bindir)/cckd2ckd$(EXEEXT)

%.s: %.c
	$(COMPILE) -S $<

%.i: %.c
	$(COMPILE) -E $< >$@

# PROGRAMMING NOTE: the following two rules cause only ieee.c and softfloat.c
# to be recompiled (the only two source files dependent on SoftFloat-specialise)
# whenever the interface to SoftFloat-specialise is changed.

.libs/SoftFloat.o:  SoftFloat-specialise.h
.libs/ieee.o:       SoftFloat-specialise.h

check:
if APPLE
	$(top_srcdir)/tests/runtest $(top_srcdir)/tests
else
	$(top_srcdir)/tests/runtest -p .libs $(top_srcdir)/tests
endif
