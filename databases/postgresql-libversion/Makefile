# Created by: Dmitry Marakasov <amdmi3@FreeBSD.org>
# $FreeBSD$

PORTNAME=	postgresql-libversion
PORTVERSION=	2.0.0
CATEGORIES=	databases

MAINTAINER=	amdmi3@FreeBSD.org
COMMENT=	PostgreSQL extension with support for version string comparison

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/COPYING

LIB_DEPENDS=	libversion.so:devel/libversion

USE_GITHUB=	yes
GH_ACCOUNT=	repology

USES=		gmake pgsql pkgconfig

PORTDOCS=	*

OPTIONS_DEFINE=	DOCS

post-install:
	@${STRIP_CMD} ${STAGEDIR}${PREFIX}/lib/postgresql/libversion.so

post-install-DOCS-on:
	@${MKDIR} ${STAGEDIR}${DOCSDIR}
	${INSTALL_DATA} ${WRKSRC}/README.md ${STAGEDIR}${DOCSDIR}/

.include <bsd.port.mk>
