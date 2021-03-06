# $FreeBSD$

PORTNAME=	ktorrent
DISTVERSION=	${KDE_APPLICATIONS_VERSION}
CATEGORIES=	net-p2p kde kde-applications

# Since libktorrent is released with ktorrent, it lives in a
# strange download directory; keep this in-sync with net-p2p/libktorrent.

MAINTAINER=	kde@FreeBSD.org
COMMENT=	BitTorrent client for KDE

USES=		cmake compiler:c++11-lang gettext kde:5 qt:5 tar:xz

LIB_DEPENDS=	libKF5Torrent.so:net-p2p/libktorrent \
		libgcrypt.so:security/libgcrypt \
		libgmp.so:math/gmp \
		libtag.so:audio/taglib
USE_KDE=	archive auth bookmarks codecs completion config configwidgets \
		coreaddons crash dbusaddons dnssd i18n iconthemes itemviews \
		jobwidgets kcmutils kio kross notifications \
		notifyconfig parts plotting service \
		solid sonnet syndication textwidgets widgetsaddons \
		windowsystem xmlgui
USE_QT=		concurrent core dbus gui network phonon4 script widgets xml \
		buildtools_build qmake_build

OPTIONS_DEFINE=	DOCS WORKSPACE
OPTIONS_DEFAULT=	WORKSPACE
OPTIONS_SUB=	yes

WORKSPACE_USE=	KDE=plasma-workspace
WORKSPACE_DESC=	Shutdown on completion support
WORKSPACE_CMAKE_BOOL_OFF=	CMAKE_DISABLE_FIND_PACKAGE_LibKWorkspace

.include <bsd.port.options.mk>

.if ${ARCH} == aarch64 || ${ARCH} == amd64 || ${ARCH} == i386
PLIST_SUB+=	WEBENGINE=""
USE_QT+=	webengine
.else
PLIST_SUB+=	WEBENGINE="@comment "
.endif

.include <bsd.port.mk>
