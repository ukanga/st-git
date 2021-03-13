# Maintainer: Tarmo Heiskanen <turskii@gmail.com>
# Contributor: mar77i <mar77i at mar77i dot ch>
# Contributor: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Scytrin dai Kinthra <scytrin@gmail.com>

pkgname=st-git
pkgver=0.8.4.r2.g4ef0cbd
pkgrel=1
pkgdesc='Simple virtual terminal emulator for X'
url='https://st.suckless.org/'
arch=('i686' 'x86_64')
license=('MIT')
depends=('libxft')
makedepends=('ncurses' 'libxext' 'git')
source=('git://git.suckless.org/st'
        st-alpha-0.8.2.diff
        st-scrollback-20200419-72e3f6c.diff
        st-scrollback-mouse-20191024-a2c479c.diff
        st-scrollback-mouse-altscreen-20200416-5703aa0.diff
	st-xresources-20200604-9ba7ecf.diff
        st-invert-0.8.2-1.diff
        st-clipboard-0.8.3.diff
        st-ligatures-alpha-scrollback-20200501-d6a8ddd.diff
        st-themed_cursor-20200501-84e4477.diff
        st-bold-is-not-bright-20190127-3be4cf1.diff
        st-anysize-0.8.1.diff
	st-dynamic-cursor-color-0.8.4.diff
        st-myconfig-20200620-3bad79d.diff
        )
sha256sums=('SKIP'
          9c5b4b4f23de80de78ca5ec3739dc6ce5e7f72666186cf4a9c6b614ac90fb285
          1e41fe17a5ef5a8194eea07422b49d815e2c2bb4d58d84771f793be423005310
          319458d980195d18fa0f81a6898d58f8d046c5ff982ab872d741f54bb60e267d
          cb87eb654985da46ff63663407184402393ad3d3013c8795570552fe56a15b9d
	  5be9b40d2b51761685f6503e92028a7858cc6571a8867b88612fce8a70514d5b
          c89bd1d6bdfb76b3301b5cf4bc66a399f35cdb64f10d3bea852a2d6aa80ef9b0
          0f5ce33953abce74a9da3088ea35bf067a9a4cfeb9fe6ea9800268ce69e436c0
          23fe795d4886f566f33eb240b38604fccba0250ca33ea919e338590ff8941e6a
          eda599b4e2c324ecfc2113c150697f3d35290a22a5926f23f08112d2d7056b13
          329169acac7ceaf901995d6e0897913089b799d8cd150c7f04c902f4a5b8eab2
          8118dbc50d2fe07ae10958c65366476d5992684a87a431f7ee772e27d5dee50f
	  c942f73cd576c2d275dea21a733bc8bcfe66fb186b86563b03d42a123fbe93b8
          378e62c33f96cfc949d06f8cd3fdf7fa74d435f154e0d35350461408860446b2
          )
provides=("st")
conflicts=("st")


pkgver() {
    cd "${srcdir}/st" || exit
    git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    cd "${srcdir}/st" || exit

    for file in "${source[@]}"; do
        if [[ "$file" == "config.h" ]]; then
            # add config.h if present in source array
            # Note: this supersedes the above sed to config.def.h
            cp "$srcdir/$file" .
        elif [[ "$file" == *.diff || "$file" == *.patch ]]; then
            # add all patches present in source array
            patch -Np1 <"$srcdir/$(basename ${file})"
        fi
    done
}

build() {
    cd "${srcdir}/st"

    make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
    cd "${srcdir}/st"

    make PREFIX=/usr DESTDIR="${pkgdir}" TERMINFO="/dev/null" install
    install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
    install -Dm644 README "${pkgdir}/usr/share/doc/${pkgname}/README"
}
