# $FreeBSD$

PORTNAME=	srsly
PORTVERSION=	2.3.2
CATEGORIES=	devel python
MASTER_SITES=	CHEESESHOP
PKGNAMEPREFIX=	${PYTHON_PKGNAMEPREFIX}

MAINTAINER=	sunpoet@FreeBSD.org
COMMENT=	Modern high-performance serialization utilities for Python

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/LICENSE

BUILD_DEPENDS=	${PYTHON_PKGNAMEPREFIX}wheel>=0:devel/py-wheel@${PY_FLAVOR}

USES=		python:3.6+
USE_PYTHON=	autoplist concurrent cython distutils

post-install:
	${STRIP_CMD} ${STAGEDIR}${PYTHON_SITELIBDIR}/srsly/*/*.so

.include <bsd.port.mk>
