# Maintainer: Tarmo Heiskanen <turskii@gmail.com>
# Contributor: mar77i <mar77i at mar77i dot ch>
# Contributor: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Scytrin dai Kinthra <scytrin@gmail.com>

pkgname=st-git
pkgver=0.8.5.r4.ge823e23
pkgrel=1
pkgdesc='Simple virtual terminal emulator for X'
url='https://st.suckless.org/'
arch=('i686' 'x86_64')
license=('MIT')
depends=('libxft')
makedepends=('ncurses' 'libxext' 'git')
source=('git://git.suckless.org/st'
        st-alpha-20220206-0.8.5.diff
        st-scrollback-20210507-4536f46.diff
        st-scrollback-mouse-20220127-2c5edf2.diff
        st-scrollback-mouse-altscreen-20220127-2c5edf2.diff
        st-xresources-20200604-9ba7ecf.diff
        st-clipboard-0.8.3.diff
        st-ligatures-alpha-scrollback-20200501-d6a8ddd.diff
        st-themed_cursor-20200501-84e4477.diff
        st-bold-is-not-bright-20190127-3be4cf1.diff
        st-anysize-20201003-407a3d0.diff
        st-dynamic-cursor-color-0.8.4.diff
        st-invert-0.8.5.diff
        st-myconfig-20220312-555adc1.diff
         )
sha256sums=('SKIP'
          42e4803ce2a67835f7e533a707a8a28e3804a26ced163145108970b9aee5fb81
          19d8f4e7fd0d1933dc6fcf6c7333db08e1b40fc75795464660c4d723eb62511c
          46ac9bcdbfeb0011533207cb0ab31657a3eb9196da1d0db346e6a9d1fc4b4f76
          8f2f17683f12d57b1c80461247fe15234f5f5a6fc52cdf48176c8358e699101d
          5be9b40d2b51761685f6503e92028a7858cc6571a8867b88612fce8a70514d5b
          0f5ce33953abce74a9da3088ea35bf067a9a4cfeb9fe6ea9800268ce69e436c0
          23fe795d4886f566f33eb240b38604fccba0250ca33ea919e338590ff8941e6a
          eda599b4e2c324ecfc2113c150697f3d35290a22a5926f23f08112d2d7056b13
          329169acac7ceaf901995d6e0897913089b799d8cd150c7f04c902f4a5b8eab2
          d968bff58e77ce9987d353b77894379fdc8d39122ae7a281d9c92909381b050d
          c942f73cd576c2d275dea21a733bc8bcfe66fb186b86563b03d42a123fbe93b8
          977c7b00d9b52664074f7ff36dd6d5d7f3efb60f6c9d0a19ed69512101125925
          555adc109f08530c7583a022a5650cb3f087f686e17d7c648c08b15bf3fd924f
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
