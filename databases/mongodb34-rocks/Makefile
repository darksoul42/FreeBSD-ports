# $FreeBSD$

PORTNAME=	mongodb
PORTVERSION=	3.4.4
DISTVERSIONPREFIX=	r
PORTREVISION=	2
CATEGORIES=	databases net
MASTER_SITES=	https://fastdl.mongodb.org/src/ \
		http://fastdl.mongodb.org/src/ \
		http://download.mongodb.org/src/
PKGNAMESUFFIX=	${PORTVERSION:R:S/.//}-rocks
DISTNAME=	mongodb-src-${DISTVERSIONPREFIX}${PORTVERSION}
DISTFILES=	${DISTNAME}${EXTRACT_SUFX}

MAINTAINER=	numisemis@yahoo.com
COMMENT=	Distributed document-oriented database with a RocksDB Storage Engine

# mongodb is AGPLv3, C++ driver is APACHE20
LICENSE=	AGPLv3 APACHE20
LICENSE_COMB=	multi

LIB_DEPENDS=	libpcre.so:devel/pcre \
		libsnappy.so:archivers/snappy \
		libboost_system.so:devel/boost-libs \
		librocksdb.so:databases/rocksdb \
		liblz4.so:archivers/liblz4
TEST_DEPENDS=	${PYTHON_PKGNAMEPREFIX}yaml>=3.11:devel/py-yaml \
		${PYTHON_PKGNAMEPREFIX}pymongo>=3.0:databases/pymongo

CHOSEN_COMPILER_TYPE=	clang

ONLY_FOR_ARCHS=	amd64
ONLY_FOR_ARCHS_REASON=	"Only supported on amd64 (i386 deprecated in v3)"

CONFLICTS_BUILD=	mongo-cxx-driver
OPTIONS_DEFINE=		SASL SSL
OPTIONS_DEFAULT=	SASL SSL

SASL_LIB_DEPENDS=	libsasl2.so:security/cyrus-sasl2
SASL_MAKE_ARGS=		--use-sasl-client

SSL_USE=	yes
SSL_MAKE_ARGS=	--ssl

USES=		compiler:c++14-lang cpe execinfo python:build scons
USE_GITHUB=	nodefault
GH_TUPLE=	mongodb-partners:mongo-rocks:r${PORTVERSION}:module/src/mongo/db/modules/rocks

WITH_DEBUG=yes
MAKE_ARGS=	--prefix=${STAGEDIR}${PREFIX} \
		--use-system-pcre --use-system-snappy \
		--use-system-boost --use-system-zlib \
		--cxx-std=14 --libc++ \
		--runtime-hardening=on \
		--disable-warnings-as-errors \
		VERBOSE=on

USERS=	mongodb
GROUPS=	mongodb

USE_RC_SUBR=	mongod \
		mongos

ALL_TARGET=	core
TEST_TARGET=	unittests

.include <bsd.port.options.mk>

.if ${PORT_OPTIONS:MSSL}
USES+=	ssl
.endif

.include <bsd.port.pre.mk>

post-install:
.for f in mongo mongod mongoperf mongos
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/bin/${f}
.endfor
	${CP} ${WRKSRC}/rpm/mongod.conf ${STAGEDIR}${PREFIX}/etc/mongod.conf.sample

do-test:
	@cd ${BUILD_WRKSRC} && ${SETENV} ${MAKE_ENV} \
		${MAKE_CMD} ${_MAKE_JOBS} ${MAKE_ARGS} ${TEST_TARGET} &&\
		${PYTHON_CMD} ${BUILD_WRKSRC}/buildscripts/resmoke.py\
		--suites=unittests --jobs=${MAKE_JOBS_NUMBER}

PORTSCOUT=	limitw:1,even

CPE_PRODUCT=	mongodb

.include <bsd.port.post.mk>
