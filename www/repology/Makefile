# Created by: Dmitry Marakasov <amdmi3@FreeBSD.org>
# $FreeBSD$

PORTNAME=	repology
PORTVERSION=	git20170317
CATEGORIES=	www

MAINTAINER=	amdmi3@FreeBSD.org
COMMENT=	Compare package versions in many repositories

LICENSE=	GPLv3+
LICENSE_FILE=	${WRKSRC}/COPYING

# Since some required python modules (Flask) are missing py3 counterparts,
# we use py- ports and require python default version 3.6+
RUN_DEPENDS=	${PYTHON_PKGNAMEPREFIX}yaml>=0:devel/py-yaml \
		${PYTHON_PKGNAMEPREFIX}Flask>=0:www/py-flask \
		${PYTHON_PKGNAMEPREFIX}rubymarshal>=0:devel/py-rubymarshal \
		${PYTHON_PKGNAMEPREFIX}requests>=0:www/py-requests \
		${PYTHON_PKGNAMEPREFIX}psycopg2>=0:databases/py-psycopg2
RUN_DEPENDS+=	wget:ftp/wget \
		git:devel/git \
		rsync:net/rsync
LIB_DEPENDS=	librpm.so:archivers/rpm4
BUILD_DEPENDS=	${LOCALBASE}/lib/libversion.a:devel/libversion

USE_GITHUB=	yes
GH_TAGNAME=	6f6a42c

USES=		pgsql:9.6+ pkgconfig python:3.6+
USE_PYTHON=	distutils autoplist

USERS=		repology
GROUPS=		repology

USERS=		www
GROUPS=		www

PLIST_SUB=	USER="${USERS}" GROUP="${GROUPS}"

.include <bsd.port.pre.mk>

.if ${PYTHON_DEFAULT} < 3.6
IGNORE=		requires DEFAULT_VERSIONS+=python=3.6 or later
.endif

.if ${PGSQL_DEFAULT} < 9.6
IGNORE=		requires DEFAULT_VERSIONS+=pgsql=9.6 or later
.endif

post-patch:
	@${REINPLACE_CMD} -e 's|repology.conf|${ETCDIR}/&|' ${WRKSRC}/repology-app.py
	@${REINPLACE_CMD} -e "s|os.path.dirname(__file__).*repology.conf|'${ETCDIR}/repology.conf|" ${WRKSRC}/repology/config.py
	@${REINPLACE_CMD} -e 's|_state|/var/db/repology|; s|repos.yaml|${ETCDIR}/&|; \
		s|rules.yaml|${ETCDIR}/&|' ${WRKSRC}/repology.conf.default

post-install:
	@${MKDIR} ${STAGEDIR}${ETCDIR}
	${INSTALL_DATA} ${WRKSRC}/repos.yaml ${STAGEDIR}${ETCDIR}/repos.yaml.sample
	${INSTALL_DATA} ${WRKSRC}/rules.yaml ${STAGEDIR}${ETCDIR}/rules.yaml.sample
	${INSTALL_DATA} ${WRKSRC}/repology.conf.default ${STAGEDIR}${ETCDIR}/repology.conf.sample

	@${MKDIR} ${STAGEDIR}/var/db/repology

.include <bsd.port.post.mk>
