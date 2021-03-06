# New ports collection makefile for:	dnsmasq
# Date created:							1 June 2003
# Whom:									Steven Honson
#
# $FreeBSD: /repoman/r/pcvs/ports/dns/dnsmasq/Makefile,v 1.34 2006/11/08 09:28:24 farrokhi Exp $

PORTNAME=	dnsmasq
PORTVERSION=	2.45
CATEGORIES=	dns ipv6
MASTER_SITES=	http://www.thekelleys.org.uk/dnsmasq/ \
		${MASTER_SITE_GENTOO}
MASTER_SITE_SUBDIR=	distfiles

MAINTAINER=	farrokhi@FreeBSD.org
COMMENT=	Lightweight, easy to configure DNS forwarder and DHCP server

DISABLE_VULNERABILITIES=yo

MAN8=		dnsmasq.8

docs=		CHANGELOG FAQ doc.html setup.html
PORTDOCS=	${docs:T}

RC_SCRIPTS_SUB=	PREFIX=${PREFIX} RC_SUBR=${RC_SUBR}

USE_GETOPT_LONG=yes
USE_GMAKE=	yes
USE_GNOME=	pkgconfig
MAKE_ENV=	LIBS="${LDFLAGS}" RPM_OPT_FLAGS="${CPPFLAGS}" COPTS="${CFLAGS}"
MAKE_ARGS=	CC="${CC}" CFLAGS="${CFLAGS}" PREFIX="${PREFIX}"

OPTIONS=	IPV6 "IPv6 support" on
OPTIONS+=	DBUS "D_BUS support" off

.include <bsd.port.pre.mk>

.if defined(WITH_DBUS)
LIB_DEPENDS+=	dbus-1:${PORTSDIR}/devel/dbus
CFLAGS+=	-DHAVE_DBUS
.endif

### We need the ISC leasefile reader
CFLAGS+=        -DHAVE_ISC_READER

USE_RC_SUBR=	dnsmasq.sh

do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/src/dnsmasq ${PREFIX}/sbin
	${INSTALL_DATA} ${WRKSRC}/dnsmasq.conf.example ${PREFIX}/etc
	${INSTALL_MAN} ${WRKSRC}/man/${PORTNAME}.8 ${PREFIX}/man/man8
.if !defined(NOPORTDOCS)
	@${MKDIR} ${DOCSDIR}
	cd ${WRKSRC} && ${INSTALL_DATA} ${docs} ${DOCSDIR}
.endif
	${MKDIR} ${EXAMPLESDIR}/dynamic-dnsmasq ${EXAMPLESDIR}/dnslist
	${INSTALL_SCRIPT} ${WRKSRC}/contrib/dynamic-dnsmasq/dynamic-dnsmasq.pl ${EXAMPLESDIR}/dynamic-dnsmasq/
	${INSTALL_SCRIPT} ${WRKSRC}/contrib/dnslist/dnslist.pl ${EXAMPLESDIR}/dnslist/
	${INSTALL_DATA} ${WRKSRC}/contrib/dnslist/dhcp.css ${EXAMPLESDIR}/dnslist/
	${INSTALL_DATA} ${WRKSRC}/contrib/dnslist/dnslist.tt2 ${EXAMPLESDIR}/dnslist/

.include <bsd.port.post.mk>
