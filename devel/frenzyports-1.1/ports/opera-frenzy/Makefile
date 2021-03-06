# New ports collection makefile for:		opera
# Date created:					November 5th 2000
# Whom:						des
#
# $FreeBSD: ports/www/opera/Makefile,v 1.72 2008/04/20 19:16:22 itetcu Exp $
#

PORTNAME=	opera
PORTVERSION=	${OPERA_VER}.${OPERA_DATE}
PKGNAMESUFFIX=	-frenzy
CATEGORIES=	www ipv6
MASTER_SITES=	ftp://opera.inode.at/unix/freebsd/${OPERA_VER:S/.//}${OPERA_MINVER}/${OPERA_REL}/en/${OPERA_LIB}/ \
		http://gd.tuwien.ac.at/infosys/browsers/opera/unix/freebsd/${OPERA_VER:S/.//}${OPERA_MINVER}/${OPERA_REL}/en/${OPERA_LIB}/ \
		http://ftp.ucr.ac.cr/WWW/opera/unix/freebsd/${OPERA_VER:S/.//}${OPERA_MINVER}/${OPERA_REL}/en/${OPERA_LIB}/ \
		http://www.microrpm.ca/pub/opera/unix/freebsd/${OPERA_VER:S/.//}${OPERA_MINVER}/${OPERA_REL}/en/${OPERA_LIB}/ \
		http://www.opera.mirroarrr.de/unix/freebsd/${OPERA_VER:S/.//}${OPERA_MINVER}/${OPERA_REL}/en/${OPERA_LIB}/ \
		http://opera.eurenet.net/unix/freebsd/${OPERA_VER:S/.//}${OPERA_MINVER}/${OPERA_REL}/en/${OPERA_LIB}/ \
		ftp://opera.ftp.fu-berlin.de/unix/freebsd/${OPERA_VER:S/.//}${OPERA_MINVER}/${OPERA_REL}/en/${OPERA_LIB}/ \
		http://ftp.ntua.gr/pub/www/Opera/unix/freebsd/${OPERA_VER:S/.//}${OPERA_MINVER}/${OPERA_REL}/en/${OPERA_LIB}/ \
		ftp://ftp.wayne.edu/opera/unix/freebsd/${OPERA_VER:S/.//}${OPERA_MINVER}/${OPERA_REL}/en/${OPERA_LIB}/ \
		http://mirrors.dedipower.com/opera/unix/freebsd/${OPERA_VER:S/.//}${OPERA_MINVER}/${OPERA_REL}/en/${OPERA_LIB}/ \
		ftp://ftp.mirror.ac.uk/sites/ftp.opera.com/unix/freebsd/${OPERA_VER:S/.//}${OPERA_MINVER}/${OPERA_REL}/en/${OPERA_LIB}/ \
		http://opera.nedmirror.nl/unix/freebsd/${OPERA_VER:S/.//}${OPERA_MINVER}/${OPERA_REL}/en/${OPERA_LIB}/ \
		ftp://ftp.tiscali.nl/pub/mirrors/opera/unix/freebsd/${OPERA_VER:S/.//}${OPERA_MINVER}/${OPERA_REL}/en/${OPERA_LIB}/ \
		${MASTER_SITE_RINGSERVER:S,%SUBDIR%,net/www/opera/unix/freebsd/${OPERA_VER:S/.//}${OPERA_MINVER}/${OPERA_REL}/en/${OPERA_LIB}/&,} \
		ftp://ftp.opera.com/pub/opera/unix/freebsd/${OPERA_VER:S/.//}${OPERA_MINVER}/${OPERA_REL}/en/${OPERA_LIB}/
DISTNAME=	${PORTNAME}-${OPERA_VER}-${OPERA_DATE}.${OPERA_TYPE}-${OPERA_LIB}-qt.i386.freebsd-en

MAINTAINER=	freebsd-maintainer@opera.com
COMMENT=	A blazingly fast, full-featured, standards-compliant browser

OPERA_VER=	9.27
OPERA_MINVER=
OPERA_REL=	final
OPERA_DATE=	20080331
OPERA_BUILD=	709

DATADIR=	${PREFIX}/share/${PORTNAME}
DOCSDIR=	${PREFIX}/share/doc/${PORTNAME}

USE_BZIP2=	yes
WANT_GNOME=	yes
ONLY_FOR_ARCHS=	i386
WRKSRC=		${WRKDIR}/${DISTNAME}-${OPERA_BUILD}

MAN1=		opera.1

NO_BUILD=	yes

OPTIONS=	ASPELL	"Use the aspell library for spell checking"	off \
		SHARED	"Use a shared Qt library (instead of static)"	off

.if exists(${LOCALBASE}/lib/libqt-mt.prl)
QT_PRESENT=	on
.else
QT_PRESENT=	off
.endif

.include <bsd.port.pre.mk>

.if ${OSVERSION} >= 700040
OPERA_TYPE=	1
.elif ${OSVERSION} >= 600000
OPERA_TYPE=	4
.else
OPERA_TYPE=	3
.endif

.if ${OSVERSION} > 700000
EXTRA_PATCHES=	${FILESDIR}/extra-patch-symhack
PLIST_SUB+=	SYMHACK=""
.else
PLIST_SUB+=	SYMHACK="@comment "
.endif

.if !defined(WITHOUT_ASPELL)
LIB_DEPENDS+=	aspell.16:${PORTSDIR}/textproc/aspell
.endif

.if defined(WITH_SHARED) && ${OPERA_TYPE} > 1
OPERA_LIB=	shared
LIB_DEPENDS+=	qt-mt:${PORTSDIR}/x11-toolkits/qt33
.else
OPERA_LIB=	static
OPERA_TYPE=	1
.endif

.if ${OPERA_TYPE} == 1
LIB_DEPENDS+=	c_r.4:${PORTSDIR}/misc/compat4x
.if ${OSVERSION} >= 600000
LIB_DEPENDS+=	z.2:${PORTSDIR}/misc/compat5x
.endif
.endif

.if ${HAVE_GNOME:Mgnomehier}!=""
PLIST_SUB+=	GNOME=""
.else
PLIST_SUB+=	GNOME="@comment "
.endif

.if exists(${LOCALBASE}/bin/kde-config)
PLIST_SUB+=	KDE=""
.else
PLIST_SUB+=	KDE="@comment "
.endif

post-patch:
	@${CP} ${WRKSRC}/search.ini ${WRKSRC}/locale/en
	@${REINPLACE_CMD} -e 's|%%PREFIX%%|${PREFIX}|g ; \
		s|%%LOCALBASE%%|${LOCALBASE}|g' \
			${WRKSRC}/install.sh ${WRKSRC}/ini/pluginpath.ini \
			${WRKSRC}/config/opera6rc
	@${REINPLACE_CMD} -e 's|%%DATADIR%%|${DATADIR}|' ${WRKSRC}/install.sh
	@${FIND} ${WRKSRC} -name '*.orig' -delete
	@${FIND} ${WRKSRC} -name '*.bak' -delete

do-install:
	@(cd ${WRKSRC} && ./install.sh --prefix=${PREFIX})

post-install:
.if ${OSVERSION} > 700000
	@${CP} ${FILESDIR}/symhack.c ${WRKSRC}/
	@(cd ${WRKSRC}/ && ${CC} -shared -o symhack.so symhack.c)
	@${INSTALL_DATA} ${WRKSRC}/symhack.so ${DATADIR}/
.endif

	@${CAT} ${PKGMESSAGE}

.include <bsd.port.post.mk>
