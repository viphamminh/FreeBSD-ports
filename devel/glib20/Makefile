# Created by: Vanilla I. Shu <vanilla@FreeBSD.org>
# $FreeBSD$

PORTNAME=	glib
DISTVERSION=	2.66.7
PORTEPOCH=	1
CATEGORIES=	devel
MASTER_SITES=	GNOME
DIST_SUBDIR=	gnome

MAINTAINER=	desktop@FreeBSD.org
COMMENT=	Some useful routines of C programming (current stable version)

LICENSE=	LGPL20

LIB_DEPENDS=	libffi.so:devel/libffi \
		libpcre.so:devel/pcre

# iconv:wchar_t - our iconv in base doesn't support utf-8 -> wchar_t (boooo)
# (wchar_t is used by glibmm, rawtherapee triggered this)
USES=		compiler:c11 gettext gnome iconv:wchar_t localbase:ldflags \
		meson perl5 pkgconfig python:3.6+ tar:xz
USE_LDCONFIG=	yes
USE_PERL5=	build
USE_PYTHON=	py3kplist
MESON_ARGS=	-Db_lundef=false \
		-Ddefault_library=both \
		-Dinstalled_tests=false \
		-Dlibmount=disabled \
		-Dselinux=disabled \
		-Dxattr=false
BINARY_ALIAS=	python3=${PYTHON_CMD}
PORTSCOUT=	limitw:1,even

_LIBVERSION=	0.6600.7
PLIST_SUB=	LIBVERSION=${_LIBVERSION}

OPTIONS_DEFINE=		DEBUG MANPAGES NLS
OPTIONS_DEFAULT=	MANPAGES
OPTIONS_SUB=		yes

MANPAGES_BUILD_DEPENDS=	docbook-xml>4.1.2:textproc/docbook-xml \
			docbook-xsl>0:textproc/docbook-xsl
MANPAGES_USE=		GNOME=libxslt:build
MANPAGES_MESON_TRUE=	man

.include <bsd.port.pre.mk>

# doesn't build yet
MESON_ARGS+=	-Ddtrace=false

.if empty(ICONV_LIB)
# native?
MESON_ARGS+=	-Diconv=libc
.else
MESON_ARGS+=	-Diconv=external
.endif

.if ${ARCH} == powerpc64
EXTRA_PATCHES=	${FILESDIR}/extra-arch-powerpc64
.endif

post-patch:
	${REINPLACE_CMD} -e 's|/usr/local|${LOCALBASE}|g ; \
		s|/usr/share/locale/locale|${LOCALBASE}/share/locale/locale|g' \
		${WRKSRC}/glib/gutils.c
	${REINPLACE_CMD} -e 's|@PYTHON@|${PYTHON_CMD}|g' \
		${WRKSRC}/gio/gdbus-2.0/codegen/gdbus-codegen.in \
		${WRKSRC}/glib/gtester-report.in \
		${WRKSRC}/gobject/glib-genmarshal.in \
		${WRKSRC}/gobject/glib-mkenums.in
	${REINPLACE_CMD} -e '/inotify_init1/d' \
		-e '/inotify.h/d' \
		${WRKSRC}/meson.build

CODEGENDIR=	${PREFIX}/share/glib-2.0/codegen

post-install:
	${MKDIR} ${STAGEDIR}${PREFIX}/share/GConf/gsettings
	${MKDIR} ${STAGEDIR}${PREFIX}/lib/gio/modules
# generates .py[co] files for installed modules
# if that's not done, ${PYTHON_SITELIBDIR}/gps will be polluted
# with these files when module is imported from root user
	${FIND} ${STAGEDIR}${PREFIX} -name \*.pyc -delete
	(cd ${STAGEDIR}${PREFIX} && \
	${PYTHON_CMD} ${PYTHON_LIBDIR}/compileall.py \
		-d ${CODEGENDIR} -f ${CODEGENDIR:S;${PREFIX}/;;} && \
	${PYTHON_CMD} -O ${PYTHON_LIBDIR}/compileall.py \
		-d ${CODEGENDIR} -f ${CODEGENDIR:S;${PREFIX}/;;})
# install bash completion regardless if bash is present
	${MKDIR} ${STAGEDIR}${PREFIX}/share/bash-completion/completions
.for file in gapplication gdbus gio gresource gsettings
	${INSTALL_DATA} ${WRKSRC}/gio/completion/${file} \
		${STAGEDIR}${PREFIX}/share/bash-completion/completions/
.endfor
	${RM} -r ${STAGEDIR}${PREFIX}/libexec/installed-tests

.include <bsd.port.post.mk>
