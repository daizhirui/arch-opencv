# Maintainer: Ray Rashif <schiv@archlinux.org>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgbase=opencv
pkgname=('opencv' 'opencv-docs' 'opencv-samples')
_realname=OpenCV
pkgver=2.4.3
pkgrel=1
pkgdesc="Open Source Computer Vision Library"
arch=('i686' 'x86_64')
license=('BSD')
url="http://opencv.org/"
depends=('jasper' 'gstreamer0.10-base' 'gtk2'
         'xine-lib' 'libdc1394' 'openexr')
makedepends=('cmake' 'python2-numpy' 'eigen2')
optdepends=('opencv-docs'
            'opencv-samples'
            'eigen2'
            'python2-numpy: Python 2.x interface')
source=("http://downloads.sourceforge.net/opencvlibrary/$_realname-${pkgver}.tar.bz2"
        'pkgconfig.patch')
md5sums=('c0a5af4ff9d0d540684c0bf00ef35dbe'
         'cb916260b5ec594fe7a0cc2e54fc569f')

_cmakeopts=('-D CMAKE_BUILD_TYPE=Release'
            '-D CMAKE_INSTALL_PREFIX=/usr'
            '-D CMAKE_SKIP_RPATH=ON'
            '-D BUILD_TESTS=OFF'
            '-D BUILD_EXAMPLES=ON'
            '-D INSTALL_C_EXAMPLES=ON'
            '-D INSTALL_PYTHON_EXAMPLES=ON'
            '-D WITH_XINE=ON'
            '-D WITH_QT=OFF'
            '-D WITH_QT_OPENGL=OFF'
            '-D WITH_UNICAP=OFF'
            '-D WITH_PVAPI=OFF'
            '-D WITH_OPENNI=OFF'
            '-D WITH_TBB=OFF'
            '-D WITH_IPP=OFF'
            '-D WITH_CUDA=OFF'
            '-D USE_FAST_MATH=ON')

build() {
  cd "$srcdir/$_realname-$pkgver"

  # fix pkg-config mess
  # see https://bugs.archlinux.org/task/32430
  patch -Np1 -i "$srcdir/pkgconfig.patch"

  cmake ${_cmakeopts[@]} .

  make
}

package_opencv() {
  cd "$srcdir/$_realname-$pkgver"

  make DESTDIR="$pkgdir" install

  # install license file
  install -Dm644 "$srcdir/$_realname-$pkgver/doc/license.txt" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  cd "$pkgdir/usr/share"

  # separate docs package; also be -R friendly
  [ -d $_realname/doc ] && mv $_realname/doc "$srcdir/opencv-doc"

  # separate samples package
  [ -d $_realname/samples ] && mv $_realname/samples "$srcdir/opencv-samples"
}

package_opencv-docs() {
  pkgdesc+=" (documentation)"
  unset depends
  unset optdepends
  options=('docs')

  cd "$srcdir"

  mkdir -p "$pkgdir/usr/share/doc"
  cp -r opencv-doc "$pkgdir/usr/share/doc/$_realname"

  # install license file
  install -Dm644 "$srcdir/$_realname-$pkgver/doc/license.txt" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

package_opencv-samples() {
  pkgdesc+=" (samples)"
  depends=('bash')
  unset optdepends
  unset options

  cd "$srcdir"

  mkdir -p "$pkgdir/usr/share/$_realname"
  cp -r opencv-samples "$pkgdir/usr/share/$_realname/samples"

  # install license file
  install -Dm644 "$srcdir/$_realname-$pkgver/doc/license.txt" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim:set ts=2 sw=2 et:
