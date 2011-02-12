# Maintainer: Ray Rashif <schiv@archlinux.org>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgname=opencv
_realname=OpenCV
pkgver=2.2.0
pkgrel=3
pkgdesc="Open Source Computer Vision Library"
arch=('i686' 'x86_64')
license=('BSD')
url="http://opencv.willowgarage.com"
depends=('jasper' 'gstreamer0.10-base' 'openexr'
         'gtk2' 'xine-lib' 'libdc1394' 'v4l-utils')
makedepends=('pkg-config' 'cmake' 'doxygen'
             'python2-numpy' 'eigen')
optdepends=('eigen'
            'python2-numpy')
options=('!libtool')
source=(http://downloads.sourceforge.net/opencvlibrary/$_realname-$pkgver.tar.bz2
        ptrcvcapture.patch)
md5sums=('122c9ac793a46854ef2819fedbbd6b1b'
         '461a8b1b0f2264521e13d9ae051d13be')

build() {
  cd "$srcdir/$_realname-$pkgver"

  # Please do not remove any patches from trunk #

  # libpng 1.4 compatibility
  #patch -Np1 -i "$srcdir/libpng-1.4.patch"

  # fix v4l issue
  #patch -Np0 -i "$srcdir/v4l-mmap.patch"

  # fix ffmpeg-related C++ issue
  # see http://code.google.com/p/ffmpegsource/source/detail?r=311
  #export CXXFLAGS="$CXXFLAGS -D__STDC_CONSTANT_MACROS"

  # fix linking against highgui
  # see https://bugs.archlinux.org/task/22841
  patch -Np0 -i "$srcdir/ptrcvcapture.patch"

  cmake . -DCMAKE_BUILD_TYPE=Release \
          -DCMAKE_INSTALL_PREFIX=/usr \
          -DCMAKE_SKIP_RPATH=ON \
          -DWITH_XINE=ON \
          -DWITH_UNICAP=OFF \
          -DBUILD_EXAMPLES=ON \
          -DBUILD_TESTS=OFF \
          -DINSTALL_C_EXAMPLES=ON \
          -DINSTALL_PYTHON_EXAMPLES=ON
  make
}

package() {
  cd "$srcdir/$_realname-$pkgver"

  make DESTDIR="$pkgdir/" install

  # install license file
  install -Dm644 "$srcdir/$_realname-$pkgver/doc/license.txt" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim:set ts=2 sw=2 et:
