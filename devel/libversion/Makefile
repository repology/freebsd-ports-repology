# Created by: Dmitry Marakasov <amdmi3@FreeBSD.org>
# $FreeBSD$

PORTNAME=	libversion
PORTVERSION=	1.0.0
CATEGORIES=	devel

MAINTAINER=	amdmi3@FreeBSD.org
COMMENT=	Advanced version string comparison library

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/COPYING

USE_GITHUB=	yes
GH_ACCOUNT=	repology

USES=		cmake
PORTDOCS=	CHANGES.md README.md TODO.md

OPTIONS_DEFINE=	DOCS

post-install-DOCS-on:
	@${MKDIR} ${STAGEDIR}${DOCSDIR}
.for f in ${PORTDOCS}
	${INSTALL_DATA} ${WRKSRC}/${f} ${STAGEDIR}${DOCSDIR}/
.endfor

.include <bsd.port.mk>
