# Maintainer: Antonio Rojas <arojas@archlinux.org>
# Contributor: Ray Rashif <schiv@archlinux.org>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgbase=opencv
pkgname=(opencv
         opencv-samples
         python-opencv
         opencv-cuda)
pkgver=4.8.0
pkgrel=6
pkgdesc='Open Source Computer Vision Library'
arch=(x86_64)
license=(BSD)
url='https://opencv.org/'
depends=(abseil-cpp
         cblas
         ffmpeg
         freetype2
         gcc-libs
         glib2
         glibc
         gst-plugins-base
         gst-plugins-base-libs
         gstreamer
         harfbuzz
         lapack
         libdc1394
         libglvnd
         libjpeg-turbo
         libpng
         libtiff
         libwebp
         openexr
         openjpeg2
         protobuf
         tbb
         zlib)
makedepends=(ant
             cmake
             cudnn
             eigen
             fmt
             glew
             hdf5
             java-environment
             lapacke
             mesa
             nlohmann-json
             openmpi
             pugixml
             python-numpy
             python-setuptools
             qt6-5compat
             vtk)
optdepends=('opencv-samples: samples'
            'vtk: for the viz module'
            'glew: for the viz module'
            'qt6-base: for the HighGUI module'
            'hdf5: for the HDF5 module'
            'opencl-icd-loader: For coding with OpenCL'
            'java-runtime: Java interface')
source=(https://github.com/opencv/opencv/archive/$pkgver/$pkgname-$pkgver.tar.gz
        https://github.com/opencv/opencv_contrib/archive/$pkgver/opencv_contrib-$pkgver.tar.gz
        vtk9.patch
        cuda-12.2.patch)
sha256sums=('cbf47ecc336d2bff36b0dcd7d6c179a9bb59e805136af6b9670ca944aef889bd'
            'b4aef0f25a22edcd7305df830fa926ca304ea9db65de6ccd02f6cfa5f3357dbb'
            'f35a2d4ea0d6212c7798659e59eda2cb0b5bc858360f7ce9c696c77d3029668e'
            '2acacd8df0fab431aa2197304c4496f3e4d8a8de9305994a6474e4c66dc3a159')

prepare() {
  patch -d $pkgname-$pkgver -p1 < vtk9.patch # Don't require all vtk optdepends
  patch -d $pkgname-$pkgver -p1 < cuda-12.2.patch # Fix build with CUDA 12.2
}

build() {
  export JAVA_HOME="/usr/lib/jvm/default"
  # cmake's FindLAPACK doesn't add cblas to LAPACK_LIBRARIES, so we need to specify them manually
  _opts="-DWITH_OPENCL=ON \
         -DWITH_OPENGL=ON \
         -DOpenGL_GL_PREFERENCE=LEGACY \
         -DCMAKE_CXX_STANDARD=17 \
         -DWITH_TBB=ON \
         -DWITH_VULKAN=ON \
         -DWITH_QT=ON \
         -DBUILD_TESTS=OFF \
         -DBUILD_PERF_TESTS=OFF \
         -DBUILD_EXAMPLES=ON \
         -DBUILD_PROTOBUF=OFF \
         -DPROTOBUF_UPDATE_FILES=ON \
         -DINSTALL_C_EXAMPLES=ON \
         -DINSTALL_PYTHON_EXAMPLES=ON \
         -DCMAKE_INSTALL_PREFIX=/usr \
         -DCPU_BASELINE_DISABLE=SSE3 \
         -DCPU_BASELINE_REQUIRE=SSE2 \
         -DOPENCV_EXTRA_MODULES_PATH=$srcdir/opencv_contrib-$pkgver/modules \
         -DOPENCV_SKIP_PYTHON_LOADER=ON \
         -DLAPACK_LIBRARIES=/usr/lib/liblapack.so;/usr/lib/libblas.so;/usr/lib/libcblas.so \
         -DLAPACK_CBLAS_H=/usr/include/cblas.h \
         -DLAPACK_LAPACKE_H=/usr/include/lapacke.h \
         -DOPENCV_GENERATE_PKGCONFIG=ON \
         -DOPENCV_ENABLE_NONFREE=ON \
         -DOPENCV_JNI_INSTALL_PATH=lib \
         -DOPENCV_GENERATE_SETUPVARS=OFF \
         -DEIGEN_INCLUDE_PATH=/usr/include/eigen3 \
         -DCMAKE_FIND_PACKAGE_PREFER_CONFIG=ON \
         -Dprotobuf_MODULE_COMPATIBLE=ON"
 
  cmake -B build -S $pkgname-$pkgver $_opts \
    -DBUILD_WITH_DEBUG_INFO=ON
  cmake --build build

  CFLAGS="${CFLAGS} -fno-lto" CXXFLAGS="${CXXFLAGS} -fno-lto" LDFLAGS="${LDFLAGS} -fno-lto" \
  cmake -B build-cuda -S $pkgname-$pkgver $_opts \
    -DBUILD_WITH_DEBUG_INFO=OFF \
    -DWITH_CUDA=ON \
    -DWITH_CUDNN=ON \
    -DCMAKE_C_COMPILER=gcc-12 \
    -DCMAKE_CXX_COMPILER=g++-12 \
    -DCUDA_ARCH_BIN='52-real;53-real;60-real;61-real;62-real;70-real;72-real;75-real;80-real;86-real;87-real;89-real;90-real;90-virtual' \
    -DCUDA_ARCH_PTX='90-virtual'
  cmake --build build-cuda
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
  pkgdesc+=' (samples)'
  depends=(opencv)
  unset optdepends

  mkdir -p "$pkgdir"/usr/share/opencv4
  mv samples "$pkgdir"/usr/share/opencv4

  # install license file
  install -Dm644 $pkgbase-$pkgver/LICENSE -t "$pkgdir"/usr/share/licenses/$pkgname
}

package_python-opencv() {
  pkgdesc='Python bindings for OpenCV'
  depends=(fmt
           glew
           hdf5
           jsoncpp
           opencv
           openmpi
           pugixml
           python-numpy
           qt6-base
           vtk)
  unset optdepends

  DESTDIR="$pkgdir" cmake --install build/modules/python3

  # install license file
  install -Dm644 $pkgbase-$pkgver/LICENSE -t "$pkgdir"/usr/share/licenses/$pkgname
}

package_opencv-cuda() {
  pkgdesc+=' (with CUDA support)'
  depends+=(cudnn)
  conflicts=(opencv)
  provides=(opencv=$pkgver)
  options=(!debug)

  DESTDIR="$pkgdir" cmake --install build-cuda

  # install license file
  install -Dm644 $pkgbase-$pkgver/LICENSE -t "$pkgdir"/usr/share/licenses/$pkgname

  # Split samples
  rm -r "$pkgdir"/usr/share/opencv4/samples

  # Add java symlinks expected by some binary blobs
  ln -sr "$pkgdir"/usr/share/java/{opencv4/opencv-${pkgver//./},opencv}.jar
  ln -sr "$pkgdir"/usr/lib/{libopencv_java${pkgver//./},libopencv_java}.so

  # Split Python bindings
  rm -r "$pkgdir"/usr/lib/python3*
}
