# Maintainer: Ray Rashif <schiv@archlinux.org>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgbase=opencv
pkgname=('opencv' 'opencv-samples')
pkgver=2.4.7
pkgrel=1
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
source=("http://downloads.sourceforge.net/opencvlibrary/$pkgname-$pkgver.tar.gz"
        'pkgconfig.patch'
        'fsh.patch')
md5sums=('33a12a8bba6e6dc32c97298c99b083b2'
         'c7cea48ed7d4f729ebdb9673bac41bd3'
         'c597598d142dd34d0eb4af7d6e9779d8')

_cmakeopts=('-D WITH_OPENCL=ON'
            '-D WITH_OPENGL=ON'
            '-D WITH_TBB=ON'
            '-D WITH_XINE=ON'
            '-D BUILD_WITH_DEBUG_INFO=OFF'
            '-D BUILD_TESTS=OFF'
            '-D BUILD_PERF_TESTS=OFF'
            '-D BUILD_EXAMPLES=ON'
            '-D INSTALL_C_EXAMPLES=ON'
            '-D INSTALL_PYTHON_EXAMPLES=ON'
            '-D CMAKE_BUILD_TYPE=Release'
            '-D CMAKE_INSTALL_PREFIX=/usr'
            '-D CMAKE_SKIP_RPATH=ON')

# SSE only available from Pentium 3 onwards (i686 is way older)
[[ "$CARCH" = 'i686' ]] && \
  _cmakeopts+=('-D ENABLE_SSE=OFF'
               '-D ENABLE_SSE2=OFF'
               '-D ENABLE_SSE3=OFF')

# all x64 CPUs support SSE2 but not SSE3
[[ "$CARCH" = 'x86_64' ]] && _cmakeopts+=('-D ENABLE_SSE3=OFF')

prepare() {
  cd "$srcdir/$pkgname-$pkgver"

  # hack-fix pkg-config mess
  # see https://bugs.archlinux.org/task/32430
  # and http://code.opencv.org/issues/1925
  patch -Np1 -i "$srcdir/pkgconfig.patch"

  # hack-fix folder naming inconsistency that they won't fix
  # see http://code.opencv.org/issues/2512
  # and https://bugs.archlinux.org/task/32342
  patch -Np1 -i "$srcdir/fsh.patch"

  # no longer including docs, see https://bugs.archlinux.org/task/34185
  # python2 compatibility for generating (html) docs
  #sed -i 's/sphinx-build/sphinx-build2/' cmake/OpenCVDetectPython.cmake
}

build() {
  cd "$srcdir/$pkgname-$pkgver"

  cmake ${_cmakeopts[@]} .

  make
}

package_opencv() {
  cd "$srcdir/$pkgname-$pkgver"

  make DESTDIR="$pkgdir" install

  # install license file
  install -Dm644 "$srcdir/$pkgname-$pkgver/doc/license.txt" \
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

  mkdir -p "$pkgdir/usr/share/opencv"
  cp -r "$srcdir/opencv-samples" "$pkgdir/usr/share/opencv/samples"

  # install license file
  install -Dm644 "$srcdir/opencv-$pkgver/doc/license.txt" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim:set ts=2 sw=2 et:
