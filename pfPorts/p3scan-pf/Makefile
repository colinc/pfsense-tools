# New ports collection makefile for:	p3scan
# Date created:				20 Februari 2003
# Whom:	      				Edwin Groothuis <edwin@mavetju.org>
#
# $FreeBSD: ports/mail/p3scan/Makefile,v 1.8 2006/04/01 11:54:35 edwin Exp $
#

PORTNAME=	p3scan-pf
PORTVERSION=	2.3.2
CATEGORIES=	mail net
MASTER_SITES=	${MASTER_SITE_SOURCEFORGE}
MASTER_SITE_SUBDIR=	${PORTNAME}
DISTFILES=	p3scan-${PORTVERSION}.tar.gz
WRKSRC=		${WRKDIR}/p3scan-${PORTVERSION}

MAINTAINER=	edwin@mavetju.org
COMMENT=	A transparent POP3-Proxy with virus-scanning capabilities

LIB_DEPENDS=	pcre.1:${PORTSDIR}/devel/pcre

USE_GMAKE=	yes
USE_GETOPT_LONG=yes
MAKE_ENV=	_LDFLAGS="${LDFLAGS} -L${LOCALBASE}/lib" GMAKE=${GMAKE}
SCRIPTS_ENV=	PKG_PREFIX=${PREFIX} NONEXISTENT=${NONEXISTENT} PASSWD=/etc/passwd
SUB_FILES=	pkg-message
PKGMESSAGE=	${WRKDIR}/pkg-message

OPTIONS=	UVSCAN "Use UVSCAN" off
OPTIONS+=	CLAMAV_DEVEL "Use clamav-devel" off
OPTIONS+=	CLAMAV "Use clamav" on

PORTDOCS=	AUTHORS CHANGELOG LICENSE NEWS README

MAN8=		p3scan.8 p3scan_readme.8
MANCOMPRESSED=	yes

.include <bsd.port.pre.mk>

.if !defined(WITHOUT_UVSCAN)
RUN_DEPENDS+=	uvscan:${PORTSDIR}/security/vscan
.endif

.ifndef(WITHOUT_CLAMAV)
CONFIGURE_ARGS+=	--enable-clamav
.	ifndef(WITHOUT_CLAMAV_LOCAL)
RUN_DEPENDS+=	clamav>=0:${PORTSDIR}/security/clamav
SUB_LIST+=	CLAMD=clamd
.	else
SUB_LIST+=	CLAMD=
.	endif
.endif
.ifdef(WITH_CLAMAV_DEVEL)
CONFIGURE_ARGS+=	--enable-clamav
.	ifndef(WITHOUT_CLAMAV_LOCAL)
RUN_DEPENDS+=	clamav-devel>=0:${PORTSDIR}/security/clamav-devel
SUB_LIST+=	CLAMD=clamd
.	else
SUB_LIST+=	CLAMD=
.	endif
.endif

LIB_DEPENDS+=	strfunc.9:${PORTSDIR}/devel/libstrfunc

do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/p3scan ${PREFIX}/sbin/
	${INSTALL_DATA} ${WRKSRC}/p3scan.conf ${PREFIX}/etc/p3scan.conf.sample
	${INSTALL_DATA} ${WRKSRC}/p3scan-en.mail ${PREFIX}/etc/p3scan.mail.sample
	${INSTALL_SCRIPT} ${FILESDIR}/p3scan.sh ${PREFIX}/etc/rc.d/
.for f in ${MAN8}
	${INSTALL_MAN} ${WRKSRC}/${f}.gz ${MAN8PREFIX}/man/man8/
.endfor
.ifndef (NOPORTDOCS)
	${MKDIR} ${DOCSDIR}
.for f in ${PORTDOCS}
	${INSTALL_DATA} ${WRKSRC}/${f} ${DOCSDIR}
.endfor
.endif

post-install:
	@${SETENV} ${SCRIPTS_ENV} ${SH} ${PKGINSTALL} ${PKGNAME} POST-INSTALL
	@${CAT} ${PKGMESSAGE}

.include <bsd.port.post.mk>
