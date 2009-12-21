# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgname=opencv
pkgver=2.0.0
pkgrel=1
pkgdesc="Intel(R) Open Source Computer Vision Library"
arch=('i686' 'x86_64')
license=('custom')
url="http://opencv.willowgarage.com"
depends=('jasper' 'gtk2' 'python')
options=('!libtool')
source=(http://downloads.sourceforge.net/opencvlibrary/OpenCV-${pkgver}.tar.bz2)
md5sums=('ed27520514baacc5edad7b06ad24130e')

build() {
  cd ${srcdir}/OpenCV-${pkgver}
  ./configure --prefix=/usr || return 1
  make || return 1
  make DESTDIR=$pkgdir install || return 1
  # install license file
  install -D -m644 COPYING ${pkgdir}/usr/share/licenses/$pkgname/LICENSE || return 1
}
