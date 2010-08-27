# Maintainer: Ray Rashif <schiv@archlinux.org>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgname=opencv
_pybin=python2
_pydir=python2.7
pkgver=2.1.0
pkgrel=2
pkgdesc="Intel(R) Open Source Computer Vision Library"
arch=('i686' 'x86_64')
license=('BSD')
url="http://opencv.willowgarage.com"
depends=('jasper' 'python2' 'gstreamer0.10-base'
          'gtk2' 'xine-lib' 'libdc1394' 'v4l-utils')
makedepends=('pkg-config' 'cmake')
options=('!libtool')
source=(http://downloads.sourceforge.net/opencvlibrary/OpenCV-$pkgver.tar.bz2
        libpng-1.4.patch
        v4l-mmap.patch)
md5sums=('1d71584fb4e04214c0085108f95e24c8'
         '301f9108f2ee875d889c279adadb254e'
         'c0e2d8ecba3b56974ea2169f61e4905f')
sha1sums=('8e2d6b653296364ab2ca0fa5a5e6b0edfca114a7'
          '4de3772ac5e33c2b0050d1f7e6678027898d8545'
          '981e4614b5360351426990e2e12d276b04b7043a')

build() {
  cd "$srcdir/OpenCV-$pkgver"

  # libpng 1.4 compatibility
  patch -Np1 -i ../libpng-1.4.patch

  # fix v4l issue
  patch -Np0 -i ../v4l-mmap.patch

  # fix ffmpeg-related C++ issue
  # see http://code.google.com/p/ffmpegsource/source/detail?r=311
  export CXXFLAGS="$CXXFLAGS -D__STDC_CONSTANT_MACROS"

  cmake . -DCMAKE_BUILD_TYPE=Release \
          -DCMAKE_INSTALL_PREFIX=/usr \
          -DCMAKE_SKIP_RPATH=ON \
          -DWITH_XINE=ON \
          -DWITH_UNICAP=OFF \
          -DPYTHON_EXECUTABLE=/usr/bin/$_pybin \
          -DPYTHON_INCLUDE_DIR=/usr/include/$_pydir \
          -DPYTHON_LIBRARY=/usr/lib/lib$_pydir.so

  make
}

package() {
  cd "$srcdir/OpenCV-$pkgver"

  make DESTDIR="$pkgdir/" install

  # install license file
  install -Dm644 "$srcdir/OpenCV-$pkgver/doc/license.txt" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim:set ts=2 sw=2 et:
