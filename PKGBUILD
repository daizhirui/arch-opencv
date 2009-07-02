# Maintainer: Tobias Powalowski <tpowa@archlinux.org>

pkgname=opencv
pkgver=1.1pre1
pkgrel=2
pkgdesc="Intel(R) Open Source Computer Vision Library"
arch=(i686 x86_64)
license=(custom)
source=(http://downloads.sourceforge.net/sourceforge/opencvlibrary/${pkgname}-${pkgver}.tar.gz
        opencv-1.0.0-gcc44.patch)
url="http://opencv.willowgarage.com"
depends=('jasper>=1.900.1-3' 'gtk2>=2.16.2-2' 'python')
options=(!libtool)
md5sums=('b147b7cd3c059831c415c5a2bcecdf95'
         '2d787a389b6fd47e181d3996374a0100')

build() {
    cd ${srcdir}/${pkgname}-1.1.0
    patch -Np1 -i ${srcdir}/opencv-1.0.0-gcc44.patch || return 1
    ./configure --prefix=/usr || return 1
    make || return 1
    make DESTDIR=$pkgdir install || return 1
    # install license file
    install -D -m644 COPYING ${pkgdir}/usr/share/licenses/opencv/COPYING
}

