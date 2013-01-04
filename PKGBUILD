# Maintainer: Ray Rashif <schiv@archlinux.org>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgbase=opencv
pkgname=('opencv' 'opencv-docs' 'opencv-samples')
_realname=OpenCV
pkgver=2.4.3
pkgrel=4
pkgdesc="Open Source Computer Vision Library"
arch=('i686' 'x86_64')
license=('BSD')
url="http://opencv.org/"
depends=('jasper' 'gstreamer0.10-base' 'intel-tbb' 'libcl'
         'xine-lib' 'libdc1394' 'openexr' 'gtkglext')
makedepends=('cmake' 'python2-numpy' 'mesa'
             'opencl-headers' 'eigen2')
optdepends=('opencv-docs'
            'opencv-samples'
            'eigen2'
            'python2-numpy: Python 2.x interface')
source=("http://downloads.sourceforge.net/opencvlibrary/$_realname-${pkgver}.tar.bz2"
        'pkgconfig.patch'
        'fsh.patch')
md5sums=('c0a5af4ff9d0d540684c0bf00ef35dbe'
         'cb916260b5ec594fe7a0cc2e54fc569f'
         '35256e3ccace373feba8131d1540a0de')

_cmakeopts=('-D WITH_OPENCL=ON'
            '-D WITH_OPENGL=ON'
            '-D WITH_TBB=ON'
            '-D WITH_XINE=ON'
            '-D ENABLE_SSE=OFF'
            '-D ENABLE_SSE2=OFF'
            '-D ENABLE_SSE3=OFF'
            '-D BUILD_PACKAGE=OFF'
            '-D BUILD_WITH_DEBUG_INFO=OFF'
            '-D BUILD_TESTS=OFF'
            '-D BUILD_PERF_TESTS=OFF'
            '-D BUILD_EXAMPLES=ON'
            '-D INSTALL_C_EXAMPLES=ON'
            '-D INSTALL_PYTHON_EXAMPLES=ON'
            '-D CMAKE_BUILD_TYPE=Release'
            '-D CMAKE_INSTALL_PREFIX=/usr'
            '-D CMAKE_SKIP_RPATH=ON')

build() {
  cd "$srcdir/$_realname-$pkgver"

  # fix pkg-config mess
  # see https://bugs.archlinux.org/task/32430
  patch -Np1 -i "$srcdir/pkgconfig.patch"

  # fix another upstream mess
  # see http://code.opencv.org/issues/2512
  patch -Np1 -i "$srcdir/fsh.patch"

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
  [ -d doc/$pkgname ] && mv doc/$pkgname "$srcdir/opencv-doc" \
    && rm -r doc

  # separate samples package
  [ -d $pkgname/samples ] && mv $pkgname/samples "$srcdir/opencv-samples"
}

package_opencv-docs() {
  pkgdesc+=" (documentation)"
  unset depends
  unset optdepends
  options=('docs')

  cd "$srcdir"

  mkdir -p "$pkgdir/usr/share/doc"
  cp -r opencv-doc "$pkgdir/usr/share/doc/opencv"

  # install license file
  #install -Dm644 "$srcdir/$_realname-$pkgver/doc/license.txt" \
  #  "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

package_opencv-samples() {
  pkgdesc+=" (samples)"
  depends=('bash')
  unset optdepends
  unset options

  cd "$srcdir"

  mkdir -p "$pkgdir/usr/share/opencv"
  cp -r opencv-samples "$pkgdir/usr/share/opencv/samples"

  # Do we really need licenses for these minor splits?
  # install license file
  #install -Dm644 "$srcdir/$_realname-$pkgver/doc/license.txt" \
  #  "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim:set ts=2 sw=2 et:
