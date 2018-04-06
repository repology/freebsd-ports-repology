# Created by: Dmitry Marakasov <amdmi3@FreeBSD.org>
# $FreeBSD$

PORTNAME=	repology
PORTVERSION=	git20180406
CATEGORIES=	www

MAINTAINER=	amdmi3@FreeBSD.org
COMMENT=	Compare package versions in many repositories

LICENSE=	GPLv3+
LICENSE_FILE=	${WRKSRC}/COPYING

# Since some required python modules (Flask) are missing py3 counterparts,
# we use py- ports and require python default version 3.6+
RUN_DEPENDS=	${PYTHON_PKGNAMEPREFIX}yaml>=0:devel/py-yaml@${FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}Flask>=0:www/py-flask@${FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}rubymarshal>=0:devel/py-rubymarshal@${FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}requests>=0:www/py-requests@${FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}psycopg2>=0:databases/py-psycopg2@${FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}lxml>=0:devel/py-lxml@${FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}libversion>=0:devel/py-libversion@${FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}pytz>=0:devel/py-pytz@${FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}pillow>=0:graphics/py-pillow@${FLAVOR}
RUN_DEPENDS+=	wget:ftp/wget \
		git:devel/git \
		rsync:net/rsync \
		${LOCALBASE}/lib/tcllib/json/json_write.tcl:devel/tcllib
LIB_DEPENDS=	librpm.so:archivers/rpm4
BUILD_DEPENDS=	${LOCALBASE}/lib/libversion.a:devel/libversion

USE_GITHUB=	yes
GH_TAGNAME=	81aa7bd
GH_TUPLE=	repology:repology-rules:595a44e:rules/rules.d

USES=		pgsql:9.6+ pkgconfig python:3.6+ shebangfix tcl
SHEBANG_FILES=	*.py test/*.py
USE_PYTHON=	distutils autoplist

# More correct, but needs UIDs/GIDs entry
#USERS=		repology
#GROUPS=		repology

USERS=		www
GROUPS=		www

PLIST_SUB=	USER="${USERS}" GROUP="${GROUPS}"

PORTDATA=	*

.include <bsd.port.pre.mk>

.if ${PYTHON_DEFAULT} < 3.6
IGNORE=		requires DEFAULT_VERSIONS+=python=3.6 or later
.endif

.if ${PGSQL_DEFAULT} < 9.6
IGNORE=		requires DEFAULT_VERSIONS+=pgsql=9.6 or later
.endif

post-patch:
	@${REINPLACE_CMD} -e 's|repology.conf|${ETCDIR}/&|' ${WRKSRC}/repology-app.py
	@${REINPLACE_CMD} -e "s|os.path.dirname(__file__).*repology.conf|'${ETCDIR}/repology.conf|" \
		${WRKSRC}/repology/config.py
	@${REINPLACE_CMD} \
		-e 's|"_state"|"/var/db/repology"|' \
		-e 's|"repos.d"|"${DATADIR}/repos.d"|' \
		-e 's|"rules.d"|"${DATADIR}/rules.d"|' \
		-e 's|"helpers"|"${DATADIR}/helpers"|' \
		-e 's|"tclsh"|"${TCLSH}"|' \
		${WRKSRC}/repology.conf.default
	@${REINPLACE_CMD} -e 's|python3|${python_CMD}|' ${WRKSRC}/Makefile

post-install:
	cd ${WRKSRC} && ${MAKE} test check

	@cd ${WRKSRC} && ${COPYTREE_SHARE} "repos.d rules.d helpers" ${STAGEDIR}${DATADIR}

	@${MKDIR} ${STAGEDIR}${ETCDIR}
	${INSTALL_DATA} ${WRKSRC}/repology.conf.default ${STAGEDIR}${ETCDIR}/repology.conf.sample

	@${MKDIR} ${STAGEDIR}/var/db/repology

.include <bsd.port.post.mk>
