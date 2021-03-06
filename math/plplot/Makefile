# Created by: Thomas Gellekum <tg@FreeBSD.org>
# $FreeBSD$

PORTNAME=	plplot
DISTVERSION=	5.15.0
PORTREVISION=	2
CATEGORIES=	math science
MASTER_SITES=	SF/${PORTNAME}/${PORTNAME}/${PORTVERSION}%20Source

MAINTAINER=	woodsb02@FreeBSD.org
COMMENT=	Scientific plotting package

LICENSE=	LGPL21+

BUILD_DEPENDS=	bash:shells/bash \
		${FREEFONT_DIR}/FreeMono.ttf:x11-fonts/freefont-ttf
LIB_DEPENDS=	libLASi.so:devel/lasi \
		libfontconfig.so:x11-fonts/fontconfig \
		libfreetype.so:print/freetype2 \
		libltdl.so:devel/libltdl
RUN_DEPENDS=	bash:shells/bash \
		${FREEFONT_DIR}/FreeMono.ttf:x11-fonts/freefont-ttf

USES=		compiler:c11 cmake gettext-runtime ghostscript gnome \
		localbase perl5 pathfix pkgconfig shebangfix
USE_GNOME=	cairo pango
USE_PERL5=	build

SHEBANG_FILES=	${WRKSRC}/examples/python/* \
		${WRKSRC}/examples/tcl/* \
		${WRKSRC}/examples/tk/*
SHEBANG_LANG=	sh xtk01 xtk02 xtk04
sh_OLD_CMD=	@SH_EXECUTABLE@
sh_CMD=		/bin/sh
xtk01_OLD_CMD=	@xtk01_LOCATION@
xtk01_CMD=	${DATADIR}/examples/tk/xtk01
xtk02_OLD_CMD=	@xtk02_LOCATION@
xtk02_CMD=	${DATADIR}/examples/tk/xtk02
xtk04_OLD_CMD=	@xtk04_LOCATION@
xtk04_CMD=	${DATADIR}/examples/tk/xtk04

CMAKE_ARGS=	-DENABLE_java:BOOL=OFF \
		-DENABLE_octave:BOOL=OFF \
		-DENABLE_pdl:BOOL=OFF \
		-DENABLE_ada:BOOL=OFF \
		-DENABLE_d:BOOL=OFF \
		-DENABLE_ocaml:BOOL=OFF \
		-DENABLE_pyqt4:BOOL=OFF \
		-DPL_FREETYPE_FONT_PATH:PATH="${FREEFONT_DIR}" \
		-DCMAKE_INSTALL_PKG_CONFIG_DIR:PATH="${PREFIX}/libdata/pkgconfig" \
		-DPLD_plmeta:BOOL=OFF
CFLAGS+=	-D_IS_BUILDING_PLPLOT_PORT_
USE_LDCONFIG=	yes
CMAKE_OFF=	DEFAULT_NO_QT_DEVICES
LDFLAGS+=	-pthread
DATADIR=	${PREFIX}/share/${PORTNAME}${PORTVERSION}
PLIST_SUB=	PLPLOT_VERSION=${PORTVERSION}

FREEFONT_DIR=	${LOCALBASE}/share/fonts/freefont-ttf

OPTIONS_DEFINE=		DOCS FORTRAN GD LUA PYTHON QT5 TCLTK THREADS WXGTK X11
OPTIONS_RADIO=		QHULL
OPTIONS_RADIO_QHULL=	QHULL2015 QHULL2003
OPTIONS_DEFAULT=	QHULL2015 THREADS X11
OPTIONS_SUB=		yes

FORTRAN_USES=		fortran
FORTRAN_CMAKE_BOOL=	ENABLE_f95
FORTRAN_CONFIGURE_ENV=	CMAKE_Fortran_COMPILER="${FC}"

GD_LIB_DEPENDS=		libgd.so:graphics/gd \
			libpng.so:graphics/png
GD_USES=		jpeg
GD_CMAKE_BOOL=		PLD_png

LUA_USES=		lua
LUA_BUILD_DEPENDS=	swig3.0:devel/swig30
LUA_CMAKE_BOOL=		ENABLE_lua
LUA_CMAKE_ON=		-DLUA_EXECUTABLE:FILEPATH="${LUA_CMD}"

PYTHON_USES=		python:2.7
PYTHON_BUILD_DEPENDS=	swig3.0:devel/swig30 \
			${PYNUMPY}
PYTHON_BINARY_ALIAS=	swig=swig3.0
PYTHON_RUN_DEPENDS=	${PYNUMPY}
PYTHON_CMAKE_BOOL=	ENABLE_python
PYTHON_CONFIGURE_ENV=	PYTHON_VERSION="${PYTHON_VERSION}"

QHULL2003_DESC=		Legacy Qhull support
QHULL2015_DESC=		Latest Qhull support
QHULL2015_LIB_DEPENDS=	libqhull.so.7:math/qhull
QHULL2003_LIB_DEPENDS=	libqhull.so.5:math/qhull5
QHULL2015_CMAKE_ON=	-DQHULL_INCLUDE_DIRS:PATH="${LOCALBASE}/include/libqhull"

TCLTK_LIB_DEPENDS=	libitk.so:x11-toolkits/itk
TCLTK_RUN_DEPENDS=	iwidgets>0:x11-toolkits/iwidgets
TCLTK_USES=		tk
TCLTK_CMAKE_BOOL=	ENABLE_itcl ENABLE_itk ENABLE_tcl ENABLE_tk
TCLTK_IMPLIES=		X11
ITCL_VER=		3.4.1
ITK_VER=		3.3
IWIDGETS_VER=		4.0.1
TCLTK_CMAKE_ON=		-DPLPLOT_TK_VERSION:STRING="${TK_VER}" \
			-DPLPLOT_ITCL_VERSION:STRING="${ITCL_VER}" \
			-DPLPLOT_ITK_VERSION:STRING="${ITK_VER}" \
			-DIWIDGETS_VERSIONS_LIST:STRING="${IWIDGETS_VER};${ITK_VER};${ITCL_VER}" \
			-DTCL_TCLSH:FILEPATH="${TCLSH}" \
			-DTCL_INCLUDE_PATH:PATH="${TCL_INCLUDEDIR}" \
			-DITCL_INCLUDE_PATH:PATH="${LOCALBASE}/include/itcl${ITCL_VER}" \
			-DITCL_LIBRARY:FILEPATH="${LOCALBASE}/lib/libitcl.so" \
			-DTK_INCLUDE_PATH:PATH="${TK_INCLUDEDIR}" \
			-DITK_INCLUDE_PATH:PATH="${LOCALBASE}/include/itk${ITK_VER}" \
			-DITK_LIBRARY:FILEPATH="${LOCALBASE}/lib/libitk.so"

THREADS_CMAKE_BOOL=	THREADS_HAVE_PTHREAD_ARG

QT5_USES=		qt:5
QT5_USE=		QT=core,gui,printsupport,svg,xml,widgets,buildtools_build,qmake_build
QT5_CMAKE_BOOL=		ENABLE_qt

WXGTK_LIB_DEPENDS=	libagg.so:graphics/agg
WXGTK_USE=		WX=3.0+
WXGTK_CMAKE_BOOL=	ENABLE_wxwidgets
WXGTK_CMAKE_ON=		-DwxWidgets_CONFIG_EXECUTABLE:FILEPATH="${WX_CONFIG}"

X11_USES=		xorg
X11_USE=		XORG=ice,sm,x11,xext
X11_CMAKE_BOOL=		PLD_xcairo PLD_xwin

post-patch:
	${REINPLACE_CMD} -e \
		's|dl dlopen|c dlopen|' \
		${WRKSRC}/cmake/modules/FindLTDL.cmake
	${GREP} -lR "/usr/local" ${WRKSRC}/cmake/modules | ${XARGS} \
		${REINPLACE_CMD} -e 's|/usr/local|${LOCALBASE}|g'

.include <bsd.port.mk>
