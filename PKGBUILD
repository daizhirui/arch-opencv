# Maintainer: Ray Rashif <schiv@archlinux.org>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgbase=opencv
pkgname=('opencv' 'opencv-samples')
pkgver=2.4.12.2
pkgrel=3
pkgdesc="Open Source Computer Vision Library"
arch=('i686' 'x86_64')
license=('BSD')
url="http://opencv.org/"
depends=('intel-tbb' 'openexr' 'xine-lib' 'libdc1394' 'gtkglext')
makedepends=('cmake' 'python2-numpy' 'mesa' 'eigen2')
optdepends=('opencv-samples'
            'eigen2'
            'libcl: For coding with OpenCL'
            'python2-numpy: Python 2.x interface')
#source=("http://downloads.sourceforge.net/opencvlibrary/$pkgname-$pkgver.zip")
source=("$pkgbase-$pkgver::https://github.com/Itseez/opencv/archive/$pkgver.zip" opencv-ffmpeg3.patch)
md5sums=('5f7cde7f0bccd90542805ea20687fc8d'
         'c752f3e83ebb021171fdd04aa7fb2e5a')

_cmakeopts=('-D WITH_OPENCL=ON'
            '-D WITH_OPENGL=ON'
            '-D WITH_TBB=ON'
            '-D WITH_XINE=ON'
            '-D WITH_GSTREAMER=OFF'
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
  cd $pkgname-$pkgver
# Fix build with ffmpeg 3.0 (Debian)
  patch -p1 -i ../opencv-ffmpeg3.patch
}

build() {
  cd "$srcdir/$pkgname-$pkgver"

  cmake ${_cmakeopts[@]} .

  make
}

package_opencv() {
  options=('staticlibs')

  cd "$srcdir/$pkgname-$pkgver"

  make DESTDIR="$pkgdir" install

  # install license file
  install -Dm644 "$srcdir/$pkgname-$pkgver/LICENSE" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  cd "$pkgdir/usr/share"

  # separate samples package; also be -R friendly
  if [[ -d OpenCV/samples ]]; then
    mv OpenCV/samples "$srcdir/$pkgname-samples"
    mv OpenCV $pkgname # otherwise folder naming is inconsistent
  elif [[ ! -d OpenCV ]]; then
    warning "Directory naming issue; samples package may not be built!"
  fi
}

package_opencv-samples() {
  pkgdesc+=" (samples)"
  depends=("opencv=$pkgver") # sample codes change with lib/API
  unset optdepends

  mkdir -p "$pkgdir/usr/share/opencv"
  cp -r "$srcdir/opencv-samples" "$pkgdir/usr/share/opencv/samples"

  # install license file
  install -Dm644 "$srcdir/opencv-$pkgver/LICENSE" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim:set ts=2 sw=2 et:
