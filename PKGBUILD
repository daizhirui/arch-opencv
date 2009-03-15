# Maintainer: Tobias Powalowski <tpowa@archlinux.org>

pkgname=opencv
pkgver=1.1pre1
pkgrel=1
pkgdesc="Intel(R) Open Source Computer Vision Library"
arch=(i686 x86_64)
license=(custom)
source=(http://dl.sourceforge.net/sourceforge/opencvlibrary/${pkgname}-${pkgver}.tar.gz)
url="http://opencv.willowgarage.com"
depends=('jasper' 'gtk2' 'python')
options=(!libtool)

build() {
    cd ${srcdir}/${pkgname}-1.1.0
    ./configure --prefix=/usr || return 1
    make || return 1
    make DESTDIR=$pkgdir install || return 1
    # install license file
    install -D -m644 COPYING ${pkgdir}/usr/share/licenses/opencv/COPYING
}
md5sums=('b147b7cd3c059831c415c5a2bcecdf95')
