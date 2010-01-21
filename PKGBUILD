# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgname=opencv
pkgver=2.0.0
pkgrel=4
pkgdesc="Intel(R) Open Source Computer Vision Library"
arch=('i686' 'x86_64')
license=('custom')
url="http://opencv.willowgarage.com"
depends=('jasper' 'gtk2' 'python' 'libtiff' 'libpng' 'xine-lib' 'gstreamer0.10-base' 'libdc1394' 'libv4l')
makedepends=('cmake' 'pkgconfig')
options=('!libtool')
source=(http://downloads.sourceforge.net/opencvlibrary/OpenCV-${pkgver}.tar.bz2 libpng-1.4.patch)
md5sums=('ed27520514baacc5edad7b06ad24130e' '301f9108f2ee875d889c279adadb254e')
sha1sums=('2090c2aaff77c3b0cc669c2ee81bea389db0a366' '4de3772ac5e33c2b0050d1f7e6678027898d8545')

build() {
  cd ${srcdir}
  patch -p0 < libpng-1.4.patch || return 1
  mkdir build
  cd build
  cmake ../OpenCV-${pkgver} \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_SKIP_RPATH=ON \
    -DWITH_UNICAP=OFF || return 1
  make || return 1
  make DESTDIR=$pkgdir install || return 1

  # install license file
  install -D -m644 ${srcdir}/OpenCV-${pkgver}/doc/license.txt \
    ${pkgdir}/usr/share/licenses/$pkgname/LICENSE || return 1
}
