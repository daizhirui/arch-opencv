# Maintainer: Ray Rashif <schiv@archlinux.org>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgbase=opencv
pkgname=('opencv' 'opencv-samples')
pkgver=3.3.1
pkgrel=2
pkgdesc="Open Source Computer Vision Library"
arch=('i686' 'x86_64')
license=('BSD')
url="http://opencv.org/"
depends=('intel-tbb' 'openexr' 'xine-lib' 'libdc1394' 'gtkglext')
makedepends=('cmake' 'python-numpy' 'python2-numpy' 'mesa' 'eigen' 'hdf5')
optdepends=('opencv-samples'
            'hdf5: support for HDF5 format'
            'opencl-icd-loader: For coding with OpenCL'
            'python-numpy: Python 3 interface'
            'python2-numpy: Python 2 interface')
source=("$pkgbase-$pkgver.tar.gz::https://github.com/opencv/opencv/archive/$pkgver.zip"
        "opencv_contrib-$pkgver.tar.gz::https://github.com/opencv/opencv_contrib/archive/$pkgver.tar.gz")
sha256sums=('e59412c7e0d398113b1e454d600fcdff54cd77100a84483ac0d82bbf52496932'
            '6f3ce148dc6e147496f0dbec1c99e917e13bf138f5a8ccfc3765f5c2372bd331')

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
            '-D CMAKE_SKIP_RPATH=ON'
            #'-D WITH_IPP=ON'
            #'-D INSTALL_CREATE_DISTRIB=ON'
            )

# SSE only available from Pentium 3 onwards (i686 is way older)
[[ "$CARCH" = 'i686' ]] && _cmakeopts+=('-D CPU_BASELINE_DISABLE=SSE2')
[[ "$CARCH" = 'x86_64' ]] && _cmakeopts+=('-D CPU_BASELINE_DISABLE=SSE3 -D CPU_BASELINE_REQUIRE=SSE2')

build() {
  mkdir -p build

  cd build
  cmake ${_cmakeopts[@]} \
    -DOPENCV_EXTRA_MODULES_PATH="$srcdir/opencv_contrib-$pkgver/modules" \
    ../$pkgname-$pkgver

  make
}

package_opencv() {
  options=('staticlibs')

  cd build

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
  # fix permissions
  chmod 755 "$pkgdir"/usr/share/opencv/samples/*

  # install license file
  install -Dm644 "$srcdir/opencv-$pkgver/LICENSE" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim:set ts=2 sw=2 et:
