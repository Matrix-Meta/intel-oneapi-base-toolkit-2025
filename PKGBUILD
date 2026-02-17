# Maintainer: lanxia <lanxia@gmail>

pkgname=intel-oneapi-base-toolkit-2025.3.1
pkgver=2025.3.1
# Get magic number and magic url from
# https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html
_pkgmagic=36
_urlmagic=6caa93ca-e10a-4cc5-b210-68f385feea9e
pkgrel=1
pkgdesc="Intel oneAPI Base Toolkit for Linux"
arch=('x86_64')
url='https://software.intel.com/content/www/us/en/develop/tools/oneapi.html'
license=('LicenseRef-Intel-EULA-Developer-Tools AND LicenseRef-Intel-Simplified')
source=("${pkgname}-${pkgver}.sh::https://registrationcenter-download.intel.com/akdlm/IRC_NAS/${_urlmagic}/intel-oneapi-base-toolkit-${pkgver}.${_pkgmagic}_offline.sh")
sha256sums=('c5757a14fe2dd428528bf6dc0c5a6498c7b135e8cf4ed93635acbf3e64a90850')
depends=(level-zero-loader)
options=(!strip staticlibs)
install="intel-oneapi-base-toolkit.install"
noextract=("${pkgname}-${pkgver}.sh")
optdepends=('libnotify: VTune GUI'
            'glib2: VTune GUI'
            'gtk3: VTune GUI'
            'at-spi2-atk: VTune GUI'
            'libdrm: VTune GUI'
            'libxcb: VTune GUI'
            'libxcrypt-comapt: VTune GUI'
            'xdg-utils: VTune GUI'
            'nss: Advisor GUI')
conflicts=('intel-oneapi-base-toolkit' 'intel-oneapi-basekit')
provides=('intel-oneapi-base-toolkit' 'intel-oneapi-basekit'
          'intel-oneapi-mkl' 'intel-oneapi-dnnl' 'intel-oneapi-tbb' 'intel-oneapi-dpl'
          'intel-oneapi-ccl' 'intel-oneapi-dpcpp-cpp-compiler' 'intel-oneapi-dal' 'intel-oneapi-tcm'
          'intel-oneapi-compiler-shared-runtime-libs' 'intel-oneapi-compiler-shared-opencl-cpu'
          'intel-oneapi-compiler-shared-runtime' 'intel-oneapi-compiler-dpcpp-cpp-runtime-libs'
          'intel-oneapi-compiler-dpcpp-cpp-runtime' 'intel-oneapi-compiler-shared' 'intel-oneapi-openmp'
          'intel-oneapi-dpcpp-debugger' 'intel-oneapi-dev-utilities' 'intel-oneapi-dpcpp-cpp'
          'intel-oneapi-vpl' 'intel-oneapi-ipp' 'intel-oneapi-ippcp' 'intel-oneapi-advisor'
          'intel-oneapi-vtune' 'intel-oneapi-fpga-group' "intel-oneapi-basekit=${pkgver}")

build() {
  sh "${pkgname}-${pkgver}.sh" \
    --extract-folder "${srcdir}" --extract-only \
    --remove-extracted-files no --log "${srcdir}"/extract.log
}

package() {
  # we have to run as a user different from root
  # otherwise the installer wants to write to /opt, /var
  # which is not possible in fakeroot
  runuser -u $USER -- "${pkgname}-${pkgver}.${_pkgmagic}_offline"/install.sh \
    --silent --eula accept \
    --components all \
    --install-dir "${pkgdir}"/opt/intel/oneapi \
    --log-dir "${srcdir}"/ --ignore-errors

  # allow low level compiler libs to be found
  local _lib_path='/opt/intel/oneapi/compiler'
  local _ldso_conf="${pkgdir}"/etc/ld.so.conf.d
  install -d "${_ldso_conf}"
  echo "${_lib_path}/latest/lib" >> "${_ldso_conf}/${pkgname}.conf"
  echo "${_lib_path}/latest/opt/compiler/lib" >> "${_ldso_conf}/${pkgname}.conf"

  # Collection of licenses used in OneAPI with pointers for all toolkits
  local majmin=$(echo "${pkgver}" | cut -d. -f1,2)
  install -Dm644 "${pkgdir}/opt/intel/oneapi/licensing/latest/licensing/${majmin}/license.htm" \
                 "${pkgdir}/usr/share/licenses/${pkgname}/license.htm"
}
