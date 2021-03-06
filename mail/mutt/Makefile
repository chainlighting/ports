# Created by: Udo Schweigert
# $FreeBSD$

PORTNAME=	mutt
DISTVERSION=	1.10.0
PORTREVISION?=	0
CATEGORIES+=	mail ipv6
MASTER_SITES=	ftp://ftp.mutt.org/pub/mutt/ \
		ftp://ftp.fu-berlin.de/pub/unix/mail/mutt/ \
		https://bitbucket.org/mutt/mutt/downloads/ \
		ftp://ftp.demon.co.uk/pub/mirrors/mutt/ \
		http://www.mutt.org.ua/download/mutt-${VVV_PATCH_VERSION}/:vvv \
		http://www2.mutt.org.ua/download/mutt-${VVV_PATCH_VERSION}/:vvv
DIST_SUBDIR=	mutt
EXTRACT_ONLY=	${DISTNAME}${EXTRACT_SUFX}

PATCH_SITES+=	http://www.mutt.org.ua/download/mutt-${VVV_PATCH_VERSION}/:vvv \
		http://www2.mutt.org.ua/download/mutt-${VVV_PATCH_VERSION}/:vvv \
		http://vc.org.ua/mutt/:vc
PATCH_DIST_STRIP=	-p1

MAINTAINER?=	dereks@lifeofadishwasher.com
COMMENT?=	Small but powerful text based program for read/writing e-mail

LICENSE=	GPLv2+

CONFLICTS?=	ja-mutt-[0-9]* ja-mutt-devel-[0-9]* mutt-1.4* mutt-devel-lite-[0-9]* mutt-lite-[0-9]* zh-mutt-devel-[0-9]*

DOCSDIR?=	${PREFIX}/share/doc/mutt
EXAMPLESDIR?=	${PREFIX}/share/examples/mutt
DATADIR?=	${PREFIX}/share/mutt
CONFIGURE_ENV=	CC="${CC} -I${LOCALBASE}/include"
CONFIGURE_ARGS=	--disable-fcntl --with-ssl=${OPENSSLBASE} \
		--with-docdir=${DOCSDIR} --sysconfdir=${PREFIX}/etc \
		--enable-external-dotlock --enable-pop --enable-imap \
		--enable-compressed --enable-sidebar

USES+=		cpe ncurses shebangfix ssl
SHEBANG_FILES=	doc/gen-map-doc smime_keys.pl contrib/smime_keys_test.pl

USE_HARDENING=	cfi safestack

.if defined(MUTT_CONFIGURE_ARGS)
CONFIGURE_ARGS+=	${MUTT_CONFIGURE_ARGS}
.endif

GNU_CONFIGURE=	yes

OPTIONS_SUB=	yes

.if defined (MUTT_LITE)
OPTIONS_EXCLUDE=	NLS DOCS EXAMPLES
.else
OPTIONS_DEFINE=	SASL DEBUG DOCS EXAMPLES FLOCK \
		FORCEBASE64 GPGME GREETING_PATCH HTML ICONV IDN IFDEF_PATCH \
		LOCALES_FIX MAILBOX_MANPAGES MAILDIR_MTIME_PATCH \
		NLS NNTP QUOTE_PATCH REVERSE_REPLY_PATCH SMART_DATE \
		SMIME_OUTLOOK_COMPAT SMTP URLVIEW XML

OPTIONS_SINGLE=	GSSAPI HCACHE SCREEN
OPTIONS_RADIO=	SPELL
OPTIONS_RADIO_SPELL=	ASPELL ISPELL
OPTIONS_SINGLE_GSSAPI=	GSSAPI_BASE GSSAPI_HEIMDAL GSSAPI_MIT GSSAPI_NONE
OPTIONS_SINGLE_HCACHE=	HCACHE_BDB HCACHE_TOKYOCABINET HCACHE_KYOTOCABINET HCACHE_NONE
OPTIONS_SINGLE_SCREEN=	NCURSES SLANG

SASL_DESC=	SASL authentication
DEBUG_DESC=	Debugging capabilities
FLOCK_DESC=	flock() usage
FORCEBASE64_DESC=	Option to force base64 encoding for compose
GPGME_DESC=	Gpgme interface
GREETING_PATCH_DESC=	Greeting support
HTML_DESC=	HTML documentation
IDN_DESC=	International Domain Names (implies ICONV)
IFDEF_PATCH_DESC=	ifdef feature
LOCALES_FIX_DESC=	Locales fix
MAILBOX_MANPAGES_DESC=	Install mbox.5/mmdf.5 manpages
MAILDIR_MTIME_PATCH_DESC=	Maildir mtime patch
NCURSES_DESC=	Ncurses support
NLS_DESC=	Native language support (implies ICONV)
QUOTE_PATCH_DESC=	Extended quoting
REVERSE_REPLY_PATCH_DESC=	Reverse_reply
SLANG_DESC=	SLANG support
SMART_DATE_DESC=	Dynamic date formatting with "%@"
SMIME_OUTLOOK_COMPAT_DESC=	SMIME outlook compatibility
SMTP_DESC=	SMTP relay support
HCACHE_DESC=	Header Cache support
HCACHE_BDB_DESC=	Use Berkeley DB (BDB)
HCACHE_TOKYOCABINET_DESC=	Use Tokyo Cabinet
HCACHE_KYOTOCABINET_DESC=	Use Kyoto Cabinet
HCACHE_NONE_DESC=	Disable Header Cache
URLVIEW_DESC=	Use urlview for the URL selector menu
XML_DESC=	Use XML tools for rebuilding documentation

OPTIONS_DEFAULT=	GPGME HTML HCACHE_BDB ICONV IDN \
		IFDEF_PATCH LOCALES_FIX MAILBOX_MANPAGES MAILDIR_MTIME_PATCH \
		NLS NCURSES QUOTE_PATCH REVERSE_REPLY_PATCH SMART_DATE \
		SMIME_OUTLOOK_COMPAT SASL SMTP URLVIEW XML GSSAPI_NONE
.endif

RUN_DEPENDS+=	${LOCALBASE}/etc/mime.types:misc/mime-support

.if !defined (MUTT_LITE)
ASPELL_BUILD_DEPENDS=	aspell:textproc/aspell
ASPELL_RUN_DEPENDS=	aspell:textproc/aspell
ISPELL_BUILD_DEPENDS=	ispell:textproc/aspell-ispell
ISPELL_RUN_DEPENDS=	ispell:textproc/aspell-ispell
URLVIEW_RUN_DEPENDS=	urlview:textproc/urlview
.endif

.if defined (MUTT_LITE)
PKGNAMESUFFIX=	-lite
USES+=	ncurses
PLIST_SUB+=	MAILBOX_MANPAGES=""
PLIST_SUB+=	HTML="@comment "
PLIST_SUB+=	NNTP="@comment "
# XXX bug in bpm ?
PLIST_SUB+=	NLS="@comment "
CONFIGURE_ARGS+=	--disable-nls --disable-iconv --without-idn
.endif

.ifmake makesum # for optional distfiles patchfiles
PORT_DBDIR=		/dev/null
OPTIONS_DEFAULT=	${OPTIONS_DEFINE}
.endif

DEBUG_CONFIGURE_ON=	--enable-debug

# Handle ncurses/ncurses-port/slang
SLANG_CONFIGURE_ON=	--with-slang=${LOCALBASE}
SLANG_LIB_DEPENDS=	libslang.so:devel/libslang2
SLANG_VARS=	PKGMESSAGE=${FILESDIR}/pkg-message.slang

NCURSES_USES=	ncurses

NLS_USES=	gettext
NLS_CONFIGURE_ENABLE=	nls
NLS_IMPLIES=	ICONV

# Handle GSSAPI from various places
GSSAPI_BASE_USES=	gssapi
GSSAPI_BASE_CONFIGURE_ON=	--with-gss=${GSSAPIBASEDIR} ${GSSAPI_CONFIGURE_ARGS}
GSSAPI_HEIMDAL_USES=	gssapi:heimdal
GSSAPI_HEIMDAL_CONFIGURE_ON=	--with-gss=${GSSAPIBASEDIR} ${GSSAPI_CONFIGURE_ARGS}
GSSAPI_MIT_USES=	gssapi:mit
GSSAPI_MIT_CONFIGURE_ON=	--with-gss=${GSSAPIBASEDIR} ${GSSAPI_CONFIGURE_ARGS}
GSSAPI_NONE_CONFIGURE_ON=	--without-gss

HCACHE_NONE_CONFIGURE_ON=	--disable-hcache
HCACHE_NONE_CONFIGURE_OFF=	--enable-hcache --without-qdbm --without-gdbm

HCACHE_BDB_CONFIGURE_WITH=	bdb
HCACHE_BDB_USES=	bdb:42+

HCACHE_TOKYOCABINET_CONFIGURE_WITH=	tokyocabinet
HCACHE_TOKYOCABINET_LIB_DEPENDS=	libtokyocabinet.so:databases/tokyocabinet

HCACHE_KYOTOCABINET_CONFIGURE_WITH=	kyotocabinet
HCACHE_KYOTOCABINET_LIB_DEPENDS=	libkyotocabinet.so:databases/kyotocabinet

ICONV_USES=	iconv:translit
ICONV_CONFIGURE_ON=	${ICONV_CONFIGURE_ARG}
ICONV_CONFIGURE_OFF=	--disable-iconv

ASPELL_EXTRA_PATCHES=	${PATCHDIR}/extra-patch-aspell
REVERSE_REPLY_PATCH_EXTRA_PATCHES=	${PATCHDIR}/extra-patch-reverse_reply

SASL_LIB_DEPENDS=	libsasl2.so:security/cyrus-sasl2
SASL_CONFIGURE_ON=	--with-sasl=${LOCALBASE}

SMIME_OUTLOOK_COMPAT_EXTRA_PATCHES=	${PATCHDIR}/extra-patch-smime-outlook
SMART_DATE_EXTRA_PATCHES=	${PATCHDIR}/extra-patch-smartdate

FLOCK_CONFIGURE_ENABLE=	flock

LOCALES_FIX_CONFIGURE_ON=	--enable-locales-fix

IDN_LIB_DEPENDS=	libidn2.so:dns/libidn2
IDN_CONFIGURE_WITH=	idn2
IDN_IMPLIES=	ICONV

IFDEF_PATCH_EXTRA_PATCHES=	${PATCHDIR}/extra-patch-ifdef

GPGME_LIB_DEPENDS+=	libgpgme.so:security/gpgme
GPGME_LIB_DEPENDS+=	libgpg-error.so:security/libgpg-error
GPGME_LIB_DEPENDS+=	libassuan.so:security/libassuan
GPGME_CONFIGURE_ENABLE=	gpgme

SMTP_CONFIGURE_ENABLE=	smtp

GREETING_PATCH_PATCHFILES=	mutt-${GREETING_PATCH_VERSION}.vc.greeting:vc

QUOTE_PATCH_PATCHFILES=	patch-${VVV_PATCH_VERSION}.vvv.initials.gz:vvv \
			patch-${VVV_PATCH_VERSION}.vvv.quote.gz:vvv

NNTP_BROKEN=	NNTP Patch needs updated for 1.10.x
NNTP_PATCHFILES=	patch-${VVV_PATCH_VERSION}.vvv.nntp.gz:-p1:vvv
NNTP_CONFIGURE_ON=	--enable-nntp
NNTP_VARS=	XML_NEEDED=yes
NNTP_CATEGORIES=	news
NNTP_USES=	perl5

DOCS_EXTRA_PATCHES=	${PATCHDIR}/extra-patch-doc-ref

.include <bsd.port.options.mk>

# un/comment as each VERSION is created for PORTVERSION
VVV_PATCH_VERSION=	1.9.5
GREETING_PATCH_VERSION=	1.9.5

.if !defined(VVV_PATCH_VERSION)
VVV_PATCH_VERSION=	${PORTVERSION}
.endif
.if !defined(GREETING_PATCH_VERSION)
GREETING_PATCH_VERSION=	${PORTVERSION}
.endif

LDFLAGS+=		-L${LOCALBASE}/lib -Wl,-rpath=${LOCALBASE}/lib:/usr/lib -ltinfow

post-patch-NNTP-on:
# Restore timestamps for autotools sources
	@${TOUCH} -r ${WRKSRC}/configure.ac.orig ${WRKSRC}/configure.ac
	@${TOUCH} -r ${WRKSRC}/Makefile.am.orig ${WRKSRC}/Makefile.am
.if ${PORT_OPTIONS:MMAILDIR_MTIME_PATCH}
	@${PATCH} -d ${PATCH_WRKSRC} ${PATCH_ARGS} -p1 < ${PATCHDIR}/extra-patch-maildir-mtime-nntp
.endif
.if ${PORT_OPTIONS:MFORCEBASE64}
	@${PATCH} -d ${PATCH_WRKSRC} ${PATCH_ARGS} -p1 < ${PATCHDIR}/extra-patch-forcebase64-nntp
.endif

post-patch-NNTP-off:
.if ${PORT_OPTIONS:MMAILDIR_MTIME_PATCH}
	@${PATCH} -d ${PATCH_WRKSRC} ${PATCH_ARGS} -p1 < ${PATCHDIR}/extra-patch-maildir-mtime
.endif
.if ${PORT_OPTIONS:MFORCEBASE64}
	@${PATCH} -d ${PATCH_WRKSRC} ${PATCH_ARGS} -p1 < ${PATCHDIR}/extra-patch-forcebase64
.endif

post-install:
	${INSTALL_MAN} ${FILESDIR}/mailcap.5 ${STAGEDIR}${MANPREFIX}/man/man5

post-install-NNTP-on:
	${INSTALL_DATA} ${WRKSRC}/ChangeLog.nntp ${STAGEDIR}${DOCSDIR}

WRKSRC=		${WRKDIR}/${DISTNAME:S/i$//}

pre-configure::
	@${REINPLACE_CMD} -e 's,?eimdal\*),?eimdal*|\"FreeBSD heimdal\"*),' \
		${CONFIGURE_WRKSRC}/${CONFIGURE_SCRIPT}
	@${REINPLACE_CMD} -e 's,/usr/bin/gpg,${LOCALBASE}/bin/gpg,g' \
		${WRKSRC}/contrib/gpg.rc
	@${REINPLACE_CMD} -e 's,pgp6,pgp,g' \
		${WRKSRC}/contrib/pgp6.rc
	@${REINPLACE_CMD} -E -e 's#(/samples|samples/)#share/examples/mutt#g' \
		${WRKSRC}/doc/reference.html \
		${WRKSRC}/doc/manual.html \
		${WRKSRC}/doc/manual.txt \
		${WRKSRC}/doc/Muttrc \
		${WRKSRC}/INSTALL \
		${WRKSRC}/init.h
	@${REINPLACE_CMD} -e 's#$$(docdir)/samples#${EXAMPLESDIR}#g' \
		${WRKSRC}/contrib/Makefile.in

pre-configure-HCACHE_BDB-on:
	@${REINPLACE_CMD} -E -e 's/BDB_VERSIONS="/&${BDB_LIB_NAME} db${BDB_VER} /' \
		${CONFIGURE_WRKSRC}/${CONFIGURE_SCRIPT}

.if defined (XML_NEEDED) && ${PORT_OPTIONS:MXML}
BUILD_DEPENDS+=	xsltproc:textproc/libxslt \
		${LOCALBASE}/share/xsl/docbook/html/docbook.xsl:textproc/docbook-xsl \
		lynx:www/lynx
.else
pre-build::
	@${REINPLACE_CMD} \
		-e 's,manual.html stamp-doc-rc stamp-doc-chunked manual.txt,stamp-doc-rc,' \
		${WRKSRC}/doc/Makefile
.endif

.include <bsd.port.mk>
