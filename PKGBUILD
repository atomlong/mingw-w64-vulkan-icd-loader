pkgname=mingw-w64-vulkan-icd-loader
_pkgname=Vulkan-Loader
pkgver=1.2.161
pkgrel=1
pkgdesc="Vulkan Installable Client Driver (ICD) Loader (mingw-w64)"
arch=(any)
url="https://www.khronos.org/vulkan/"
groups=("mingw-w64-vulkan-devel")
license=('APACHE')
makedepends=(mingw-w64-cmake mingw-w64-vulkan-headers python-lxml)
depends=(mingw-w64-crt)
options=(!buildflags staticlibs !strip)
source=("https://github.com/KhronosGroup/${_pkgname}/archive/v${pkgver}.tar.gz"
        "https://github.com/KhronosGroup/Vulkan-Loader/pull/498.patch"
        "https://github.com/KhronosGroup/Vulkan-Loader/pull/517.patch")
sha256sums=('3bd93560077a0ae450bb0e6d56691d91ba025435d8b2fc808fb27eecd2b6b4f6'
            'b6005ca37d8d1bbcdc95fbae56334937021b2b453985ca37e7578d8754cab2e4'
            '54eb290bf0c6f57d36e669dfb5330df1ed35a2b09ea0dc0ddc1203b8d8d5f643')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

prepare() {
  cd "${srcdir}/${_pkgname}-${pkgver}"
  patch -p1 -i "${srcdir}"/498.patch
  patch -p1 -i "${srcdir}"/517.patch
}

build() {
  cd "${srcdir}/${_pkgname}-${pkgver}"
  for _arch in ${_architectures}; do
    mkdir -p build-${_arch} && pushd build-${_arch}
    MINGW_CPPFLAGS="-D_WIN32_WINNT=0x0600" ${_arch}-cmake -DBUILD_TESTS=OFF -DUSE_MASM=OFF ..
    make
    popd
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}"/${_pkgname}-${pkgver}/build-${_arch}
    make DESTDIR="${pkgdir}" install
    ${_arch}-strip --strip-unneeded "${pkgdir}/usr/${_arch}/bin/"*.dll
    ${_arch}-strip -g "${pkgdir}/usr/${_arch}/lib/"*.a
  done
}
