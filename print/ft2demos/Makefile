# Created by: KANOU Hiroki <kanou@khdd.net>
# $FreeBSD$

PORTNAME=	ft2demos
PORTVERSION=	2.7.1
CATEGORIES=	print
MASTER_SITES=	http://savannah.nongnu.org/download/freetype/ \
		SF/freetype/freetype-demos/${PORTVERSION}

MAINTAINER=	bofh@FreeBSD.org
COMMENT=	Demo programs for FreeType2

LICENSE=	BSD3CLAUSE GPLv2
LICENSE_COMB=	dual

BUILD_DEPENDS=	${NONEXISTENT}:print/freetype2:build
LIB_DEPENDS=	libfreetype.so:print/freetype2

USES=		gmake tar:bzip2
USE_XORG=	x11
MAKE_ARGS=	EXES="${FT_PROGRAMS}" X11_PATH="${LOCALBASE}"

PLIST_FILES=	${FT_PROGRAMS:S,^,bin/,}

FT_PROGRAMS=	ftbench ftchkwd ftdiff ftdump ftgamma ftgrid ftlint ftmemchk \
		ftmulti ftpatchk ftstring fttimer ftvalid ftview

post-extract:
	${LN} -sf `${MAKE} -C ${.CURDIR}/../../print/freetype2 -V WRKSRC` ${WRKDIR}/freetype2

do-install:
	@(cd ${WRKSRC}/bin/.libs && \
		${INSTALL_PROGRAM} ${FT_PROGRAMS} ${STAGEDIR}${PREFIX}/bin)

.include <bsd.port.mk>
