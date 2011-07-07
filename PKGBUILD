# Maintainer: Ray Rashif <schiv@archlinux.org>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgbase=opencv
pkgname=('opencv' 'opencv-docs' 'opencv-samples')
_realname=OpenCV
pkgver=2.3.0
pkgrel=1
pkgdesc="Open Source Computer Vision Library"
arch=('i686' 'x86_64')
license=('BSD')
url="http://opencv.willowgarage.com/"
depends=('jasper' 'gstreamer0.10-base' 'openexr'
         'gtk2' 'xine-lib' 'libdc1394' 'v4l-utils')
makedepends=('pkg-config' 'cmake' 'python2-numpy' 'eigen2')
optdepends=('python2-numpy: Python 2.x interface'
            'eigen2')
options=('!libtool')
source=("http://downloads.sourceforge.net/opencvlibrary/$_realname-$pkgver.tar.bz2"
        'pypkgpath_nondebian.patch')
md5sums=('dea5e9df241ac37f4439da16559e420d'
         'eb884bad665047276e80556e44043a43')

_cmakeopts=('-D CMAKE_BUILD_TYPE=Release'
            '-D CMAKE_INSTALL_PREFIX=/usr'
            '-D CMAKE_SKIP_RPATH=ON'
            '-D BUILD_TESTS=OFF'
            '-D ENABLE_SSE=ON'
            '-D ENABLE_SSE2=ON'
            '-D ENABLE_SSE3=OFF'
            '-D ENABLE_SSSE3=OFF'
            '-D ENABLE_SSE41=OFF'
            '-D ENABLE_SSE42=OFF'
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

  # fix upstream's assumption of "debian-based"
  patch -Np1 -i "$srcdir/pypkgpath_nondebian.patch"

  # x64, i.e "Athlon64" and upwards, can use SSE3
  [ $CARCH = x86_64 ] && \
    _cmakeopts=${_cmakeopts[@]/ENABLE_SSE3=OFF/ENABLE_SSE3=ON}

  cmake ${_cmakeopts[@]} .

  make
}

package_opencv() {
  cd "$srcdir/$_realname-$pkgver"

  make DESTDIR="$pkgdir" install

  # install license file
  install -Dm644 "$srcdir/$_realname-$pkgver/doc/license.txt" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  # separate docs package; also be -R friendly
  [ -d "$pkgdir/usr/share/opencv/doc" ] && \
    mv "$pkgdir/usr/share/opencv/doc" "$srcdir/opencv-doc"

  # separate samples package
  [ -d "$pkgdir/usr/share/opencv/samples" ] && \
    mv "$pkgdir/usr/share/opencv/samples" "$srcdir/opencv-samples"
}

package_opencv-docs() {
  pkgdesc+=" (documentation)"
  arch=('any')
  depends=()
  optdepends=()
  options=('docs')

  cd "$srcdir"

  mkdir -p "$pkgdir/usr/share/doc"
  cp -r opencv-doc "$pkgdir/usr/share/doc/opencv"
}

package_opencv-samples() {
  pkgdesc+=" (samples)"
  arch=('any')
  depends=()
  optdepends=()
  options=()

  cd "$srcdir"

  mkdir -p "$pkgdir/usr/share/opencv"
  cp -r opencv-samples "$pkgdir/usr/share/opencv/samples"
}

# vim:set ts=2 sw=2 et:
