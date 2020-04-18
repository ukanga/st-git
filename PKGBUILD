# Maintainer: Tarmo Heiskanen <turskii@gmail.com>
# Contributor: mar77i <mar77i at mar77i dot ch>
# Contributor: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Scytrin dai Kinthra <scytrin@gmail.com>

pkgname=st-git
pkgver=0.8.2.r33.g5703aa0
pkgrel=1
pkgdesc='Simple virtual terminal emulator for X'
url='https://st.suckless.org/'
arch=('i686' 'x86_64')
license=('MIT')
depends=('libxft')
makedepends=('ncurses' 'libxext' 'git')
source=('git://git.suckless.org/st'
        'st-scrollback-20200504-28ad288.diff'
        'st-scrollback-mouse-20191024-a2c479c.diff'
        'st-xresources-20190105-3be4cf1.diff'
        'st-alpha-0.8.2.diff'
        'st-invert-0.8.2-1.diff'
        'st-clipboard-0.8.2-1.diff'
        'st-ligatures-alpha-scrollback-20200406-28ad288-1.diff'
        'st-themed_cursor-0.8.2.diff'
        'st-bold-is-not-bright-20190127-3be4cf1.diff'
        )
sha1sums=('SKIP'
          '7f0da51f25bfdff3a3a91d63f21c4b203849397b'
          'e457b4819f5233999e21d6df8438931160cd9181'
          '00d4234c5857d6067cede2a0765caabf6fa27b2d'
          'afe4f9808fc7a3fc67fcaacec6ff058ed2dcddcf'
          '97b808a486913607a41f2f30881ba599af5ecfdc'
          '7e3093ebf5250525a3cc05454c3b66b19dd383bc'
          'e28c10e654a29688d0d70cee782f884fa7374364'
          '2b1653d0c5456e13b2029ff7cc99e5a18d1d8292'
          'bef42114952e4fead262bb1b491112014ac7bc39'
          )
provides=("st")
conflicts=("st")


pkgver() {
    cd "${srcdir}/st"
    git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    cd "${srcdir}/st"

    echo 'Copying config.def.h to $startdir...'
    cp config.def.h "${startdir}/"

    echo 'Copying config.h from $startdir if it exists...'
    [ -f "${startdir}/config.h" ] && cp "${startdir}/config.h" . || true

    sed \
        -e '/char \*font/s/= .*/= "Fira Code:size=14:antialias=true:autohint=true";/' \
        -e '/wchar_t \*worddelimiters/s/= .*/= L" '"'"'`\\\"()[]{}<>|";/' \
        -e '/int defaultcs/s/= .*/= 1;/' \
        -i config.def.h
    sed \
        -e 's/CPPFLAGS =/CPPFLAGS +=/g' \
        -e 's/CFLAGS =/CFLAGS +=/g' \
        -e 's/LDFLAGS =/LDFLAGS +=/g' \
        -e 's/_BSD_SOURCE/_DEFAULT_SOURCE/' \
        -i config.mk
    sed '/@tic/d' -i Makefile

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
