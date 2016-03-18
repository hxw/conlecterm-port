# $FreeBSD$

PORTNAME=	conlecterm
PORTVERSION=	1.0
DISTVERSIONPREFIX=	v
CATEGORIES=	x11
MASTER_SITES+=	http://hackage.haskell.org/packages/archive/:hackage
DIST_SUBDIR=	${PORTNAME}
EXTRACT_ONLY=	${DISTNAME}${EXTRACT_SUFX}

MAINTAINER=	hsw@ms2.hinet.net
COMMENT=	Multi tabbed terminal for rxvt/xterm/emacs

LICENSE=	BSD3CLAUSE
LICENSE_FILE=	${WRKSRC}/LICENSE.text

BUILD_DEPENDS=	cabal:${PORTSDIR}/devel/hs-cabal-install
RUN_DEPENDS=	${LOCALBASE}/bin/urxvt:${PORTSDIR}/x11/rxvt-unicode

USE_GITHUB=	yes
GH_ACCOUNT=	hxw
#GH_TAGNAME=	92c22ed

USE_XORG=	x11
USE_GNOME=      glib20 gtk20 libxml2 pango

PLIST_FILES=	bin/conlecterm share/applications/conlecterm.desktop
PORTEXAMPLES=	config.rc.sample

# from Conlecterm.cabal
CONLECTERM_DEPS= \
		QuickCheck==2.8.2 \
		X11==1.6.1.2 \
		aeson==0.11.1.0 \
		alex==3.1.7 \
		attoparsec==0.13.0.1 \
		cairo==0.13.1.1 \
		data-default-class==0.0.1 \
		data-default-instances-base==0.0.1 \
		data-default-instances-containers==0.0.1 \
		data-default-instances-dlist==0.0.1 \
		data-default-instances-old-locale==0.0.1 \
		data-default==0.5.3 \
		dlist==0.7.1.2 \
		fail==4.9.0.0 \
		gio==0.13.1.1 \
		glib==0.13.2.2 \
		gtk==0.14.2 \
		gtk2hs-buildtools==0.13.0.5 \
		happy==1.19.5 \
		hashable==1.2.4.0 \
		hashtables==1.2.1.0 \
		pango==0.13.1.1 \
		primitive==0.6.1.0 \
		scientific==0.3.4.6 \
		semigroups==0.18.1 \
		tagged==0.8.3 \
		tf-random==0.5 \
		unordered-containers==0.2.7.0 \
		utf8-string==1.0.1.1 \
		vector==0.11.0.0

.for _dep in ${CONLECTERM_DEPS}
DISTFILES+=	package/${_dep:S/==/-/}.tar.gz:hackage
.endfor

CABAL_REPO=	${WRKSRC}/local-repo
post-extract:
	@${MKDIR} '${CABAL_REPO}' '${CABAL_REPO}/index' '${CABAL_REPO}/index2'
	@${LN} -s '${_DISTDIR}/package' '${CABAL_REPO}/'
	@cd '${CABAL_REPO}/' && \
	 for p in package/* ; \
	 do \
	   tar xf "$${p}" -C '${CABAL_REPO}/index' '*.cabal' ; \
	 done && \
	 for p in '${CABAL_REPO}/index'/* ; \
	 do \
	   [ -d "$${p}" ] || continue ; \
	   name_version=$$(basename "$${p}" | ${SED} -E 's@^(.+)-([.[:digit:]]+)$$@\1/\2@') ; \
	   ${MKDIR} "${CABAL_REPO}/index2/$${name_version}" ; \
	   ${MV} "$${p}"/*.cabal "${CABAL_REPO}/index2/$${name_version}/" ; \
	 done && \
	 tar czf 00-index.tar.gz -C '${CABAL_REPO}/index2' .

do-build:
	@cd '${WRKSRC}' && \
	 ${ECHO} 'remote-repo: hackage.haskell.org:file://${WRKSRC}/local-repo' > cabal.config && \
	 ${ECHO} 'remote-repo-cache: local-cache' >> cabal.config && \
	 ${SETENV} HOME=${WRKDIR} ${BSDMAKE} 'all'

do-install:
	@${MKDIR} ${STAGEDIR}${PREFIX}${DESKTOPDIR}
	@cd '${WRKSRC}' && \
	 ${SETENV} HOME=${WRKDIR} ${BSDMAKE} INSTALL_DIR='${STAGEDIR}${PREFIX}/bin/' APPLICATIONS_DIR='${STAGEDIR}${DESKTOPDIR}' 'install'
	@${MKDIR} ${STAGEDIR}${EXAMPLESDIR}
	${INSTALL_DATA} ${WRKSRC}/config.rc ${STAGEDIR}${EXAMPLESDIR}/config.rc.sample

.include <bsd.port.mk>
