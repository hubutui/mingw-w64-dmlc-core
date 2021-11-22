# Maintainer: Hu Butui <hot123tea123@gmail.com>

_realname=dmlc-core
pkgbase=mingw-w64-${_realname}
pkgname=("${MINGW_PACKAGE_PREFIX}-${_realname}")
pkgver=0.3
pkgrel=1
pkgdesc="A common bricks library for building scalable and portable distributed machine learning"
arch=('any')
mingw_arch=('mingw32' 'mingw64' 'ucrt64' 'clang64' 'clang32')
url="https://github.com/dmlc/dmlc-core"
license=('Apache')
depends=(
  "${MINGW_PACKAGE_PREFIX}-gcc-libs"
)
makedepends=("${MINGW_PACKAGE_PREFIX}-cmake")
source=("${_realname}-${pkgver}.tar.gz::https://github.com/dmlc/dmlc-core/archive/refs/tags/v${pkgver}.tar.gz")
sha256sums=('29a6bac5b1639d079cb7336b15d0ea99dbd72dda9cac62057b652d871d06753b')

prepare() {
  # skip test
  sed -i "/add_subdirectory(test\/unittest)/d" "${_realname}-${pkgver}/CMakeLists.txt"
}

build() {
  echo "==> Build static version"
  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
  ${MINGW_PREFIX}/bin/cmake.exe \
    -B "${srcdir}/build-static-${MINGW_CHOST}" \
    -DBUILD_SHARED_LIBS=OFF \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=${MINGW_PREFIX} \
    -DUSE_CXX14_IF_AVAILABLE=ON \
    -G'MSYS Makefiles' \
    -S ${_realname}-${pkgver}
  ${MINGW_PREFIX}/bin/cmake.exe --build "${srcdir}/build-static-${MINGW_CHOST}"

  echo "==> Build shared version"
  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
  ${MINGW_PREFIX}/bin/cmake.exe \
    -B "${srcdir}/build-shared-${MINGW_CHOST}" \
    -DBUILD_SHARED_LIBS=ON \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=${MINGW_PREFIX} \
    -DUSE_CXX14_IF_AVAILABLE=ON \
    -G'MSYS Makefiles' \
    -S ${_realname}-${pkgver}
  ${MINGW_PREFIX}/bin/cmake.exe --build "${srcdir}/build-shared-${MINGW_CHOST}"
}

package() {
  DESTDIR="${pkgdir}" \
  ${MINGW_PREFIX}/bin/cmake.exe \
    --build "${srcdir}/build-static-${MINGW_CHOST}" \
    --target install

  DESTDIR="${pkgdir}" \
  ${MINGW_PREFIX}/bin/cmake.exe \
    --build "${srcdir}/build-shared-${MINGW_CHOST}" \
    --target install
  # manual install libdmlc.dll
  cp -vf "${srcdir}/build-shared-${MINGW_CHOST}/libdmlc.dll" "${pkgdir}/${MINGW_PREFIX}/lib"
}
