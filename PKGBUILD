# Maintainer: Antonio Rojas <arojas@archlinux.org>
# Contributor: Ray Rashif <schiv@archlinux.org>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgbase=opencv
pkgname=(opencv opencv-samples python-opencv)
pkgver=4.5.2
pkgrel=2
pkgdesc="Open Source Computer Vision Library"
arch=(x86_64)
license=(BSD)
url="https://opencv.org/"
depends=(tbb openexr gst-plugins-base libdc1394 cblas lapack libgphoto2 openjpeg2 ffmpeg)
makedepends=(cmake python-numpy python-setuptools mesa eigen hdf5 lapacke qt5-base vtk glew ant java-environment)
optdepends=('opencv-samples: samples'
            'vtk: for the viz module'
            'glew: for the viz module'
            'qt5-base: for the HighGUI module'
            'hdf5: for the HDF5 module'
            'opencl-icd-loader: For coding with OpenCL'
            'java-runtime: Java interface')
source=(https://github.com/opencv/opencv/archive/$pkgver/$pkgname-$pkgver.tar.gz
        https://github.com/opencv/opencv_contrib/archive/$pkgver/opencv_contrib-$pkgver.tar.gz
        opencv-lapack-3.9.1.patch)
sha256sums=('be976b9ef14f1deaa282fb6e30d75aa8016a2d5c1f08e85795c235148940d753'
            '9f52fd3114ac464cb4c9a2a6a485c729a223afb57b9c24848484e55cef0b5c2a'
            '5233d9b4b8e3f4600e3f4ebef2b0ad5621faf25efbdfee96ee720a83cc81d0cc')

prepare() {
  patch -d $pkgname-$pkgver -p1 < opencv-lapack-3.9.1.patch # Fix build with LAPACK 3.9.1
}

build() {
  export JAVA_HOME="/usr/lib/jvm/default"
  # cmake's FindLAPACK doesn't add cblas to LAPACK_LIBRARIES, so we need to specify them manually
  _pythonpath=`python -c "from sysconfig import get_path; print(get_path('platlib'))"`
  cmake -B build -S $pkgname-$pkgver \
    -DWITH_OPENCL=ON \
    -DWITH_OPENGL=ON \
    -DWITH_TBB=ON \
    -DWITH_VULKAN=ON \
    -DWITH_QT=ON \
    -DBUILD_WITH_DEBUG_INFO=OFF \
    -DBUILD_TESTS=OFF \
    -DBUILD_PERF_TESTS=OFF \
    -DBUILD_EXAMPLES=ON \
    -DINSTALL_C_EXAMPLES=ON \
    -DINSTALL_PYTHON_EXAMPLES=ON \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCPU_BASELINE_DISABLE=SSE3 \
    -DCPU_BASELINE_REQUIRE=SSE2 \
    -DOPENCV_EXTRA_MODULES_PATH="$srcdir/opencv_contrib-$pkgver/modules" \
    -DOPENCV_SKIP_PYTHON_LOADER=ON \
    -DOPENCV_PYTHON3_INSTALL_PATH=$_pythonpath \
    -DLAPACK_LIBRARIES="/usr/lib/liblapack.so;/usr/lib/libblas.so;/usr/lib/libcblas.so" \
    -DLAPACK_CBLAS_H="/usr/include/cblas.h" \
    -DLAPACK_LAPACKE_H="/usr/include/lapacke.h" \
    -DOPENCV_GENERATE_PKGCONFIG=ON \
    -DOPENCV_ENABLE_NONFREE=ON \
    -DOPENCV_JNI_INSTALL_PATH=lib \
    -DOPENCV_GENERATE_SETUPVARS=OFF \
    -DEIGEN_INCLUDE_PATH=/usr/include/eigen3
  cmake --build build
}

package_opencv() {
  DESTDIR="$pkgdir" cmake --install build

  # install license file
  install -Dm644 $pkgbase-$pkgver/LICENSE -t "$pkgdir"/usr/share/licenses/$pkgname

  # separate samples package
  mv "$pkgdir"/usr/share/opencv4/samples "$srcdir"

  # Add java symlinks expected by some binary blobs
  ln -sr "$pkgdir"/usr/share/java/{opencv4/opencv-${pkgver//./},opencv}.jar
  ln -sr "$pkgdir"/usr/lib/{libopencv_java${pkgver//./},libopencv_java}.so

  # Split Python bindings
  rm -r "$pkgdir"/usr/lib/python3*
}

package_opencv-samples() {
  pkgdesc+=" (samples)"
  depends=("opencv=$pkgver")
  unset optdepends

  mkdir -p "$pkgdir"/usr/share/opencv4
  mv samples "$pkgdir"/usr/share/opencv4

  # install license file
  install -Dm644 $pkgbase-$pkgver/LICENSE -t "$pkgdir"/usr/share/licenses/$pkgname
}

package_python-opencv() {
  pkgdesc="Python bindings for OpenCV"
  depends=(python-numpy opencv vtk glew qt5-base hdf5)
  unset optdepends

  DESTDIR="$pkgdir" cmake --install build/modules/python3

  # install license file
  install -Dm644 $pkgbase-$pkgver/LICENSE -t "$pkgdir"/usr/share/licenses/$pkgname
}
