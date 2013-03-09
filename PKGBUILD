# Maintainer: Ray Rashif <schiv@archlinux.org>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgbase=opencv
pkgname=('opencv' 'opencv-samples')
_realname=OpenCV
pkgver=2.4.4
pkgrel=2
pkgdesc="Open Source Computer Vision Library"
arch=('i686' 'x86_64')
license=('BSD')
url="http://opencv.org/"
depends=('jasper' 'gstreamer0.10-base' 'intel-tbb' 'libcl'
         'xine-lib' 'libdc1394' 'openexr' 'gtkglext')
makedepends=('cmake' 'python2-numpy' 'mesa'
             'opencl-headers' 'eigen2')
optdepends=('opencv-samples'
            'eigen2'
            'python2-numpy: Python 2.x interface')
source=("http://downloads.sourceforge.net/opencvlibrary/$_realname-${pkgver}.tar.bz2"
        'pkgconfig.patch'
        'fsh.patch')
md5sums=('bb7272c102a801a9f9ee01db6e7ad8e9'
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
  # and http://code.opencv.org/issues/1925
  patch -Np1 -i "$srcdir/pkgconfig.patch"

  # fix another upstream mess that they won't fix
  # see http://code.opencv.org/issues/2512
  patch -Np1 -i "$srcdir/fsh.patch"

  # no longer including docs, see https://bugs.archlinux.org/task/34185
  # python2 compatibility for generating (html) docs
  #sed -i 's/sphinx-build/sphinx-build2/' cmake/OpenCVDetectPython.cmake

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

  # separate samples package; also be -R friendly
  [[ -d $pkgname/samples ]] && mv $pkgname/samples "$srcdir/opencv-samples"
}

package_opencv-samples() {
  pkgdesc+=" (samples)"
  depends=('bash')
  unset optdepends
  unset options

  cd "$srcdir"

  mkdir -p "$pkgdir/usr/share/opencv"
  cp -r opencv-samples "$pkgdir/usr/share/opencv/samples"
}

# vim:set ts=2 sw=2 et:
