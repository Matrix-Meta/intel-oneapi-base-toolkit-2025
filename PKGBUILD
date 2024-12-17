pkgname=intel-oneapi-basekit-2025
_major_ver=2025
_minor_ver=0
_patch_ver=1
_pkgver=$_major_ver.$_minor_ver.$_patch_ver
pkgver=$_pkgver.46
conflicts=(intel-oneapi-basekit)
_urlver=dfc4a434-838c-4450-a6fe-2fa903b75aa7
pkgrel=1
pkgdesc="Intel oneAPI Base Toolkit for Linux, 2025 version"
arch=('x86_64')
url='https://software.intel.com/content/www/us/en/develop/tools/oneapi.html'
license=('LicenseRef-Intel-EULA-Developer-Tools AND LicenseRef-Intel-Simplified')
source=("https://registrationcenter-download.intel.com/akdlm/IRC_NAS/${_urlver}/intel-oneapi-base-toolkit-${pkgver}_offline.sh")
b2sums=('b6c878b56436c35fcf14a9bd68b442edfb229d659b36043c6345631b12c29fa447d339888b11670098c341c19e86549aac433548a2c266edefc46662f4f0f162')
depends=(level-zero-loader)
options=(!strip staticlibs)
install="${pkgname}.install"
noextract=("intel-oneapi-base-toolkit-${pkgver}_offline.sh")
optdepends=('libnotify: VTune GUI'
            'glib2: VTune GUI'
            'gtk3: VTune GUI'
            'at-spi2-atk: VTune GUI'
            'libdrm: VTune GUI'
            'libxcb: VTune GUI'
            'libxcrypt-comapt: VTune GUI'
            'xdg-utils: VTune GUI'
            'nss: Advisor GUI')
provides=('intel-oneapi-mkl' 'intel-oneapi-dnnl' 'intel-oneapi-tbb' 'intel-oneapi-dpl'
          'intel-oneapi-ccl' 'intel-oneapi-dpcpp-cpp-compiler' 'intel-oneapi-dal' 'intel-oneapi-tcm'
          'intel-oneapi-compiler-shared-runtime-libs' 'intel-oneapi-compiler-shared-opencl-cpu'
          'intel-oneapi-compiler-shared-runtime' 'intel-oneapi-compiler-dpcpp-cpp-runtime-libs'
          'intel-oneapi-compiler-dpcpp-cpp-runtime' 'intel-oneapi-compiler-shared' 'intel-oneapi-openmp'
          'intel-oneapi-dpcpp-debugger' 'intel-oneapi-dev-utilities' 'intel-oneapi-dpcpp-cpp'
          'intel-oneapi-vpl' 'intel-oneapi-ipp' 'intel-oneapi-ippcp' 'intel-oneapi-advisor'
          'intel-oneapi-vtune' 'intel-oneapi-fpga-group'
	  'intel-oneapi-basekit')

build() {
  cd "${srcdir}"

  sh "intel-oneapi-base-toolkit-${pkgver}_offline.sh" \
    --extract-folder "${srcdir}" --extract-only \
    --remove-extracted-files no --log "${srcdir}"/extract.log
}

package() {
  cd "${srcdir}"

  # we have to run as a user different from root
  # otherwise the installer wants to write to /opt, /var
  # which is not possible in fakeroot
  runuser -u builduser -- "${srcdir}/intel-oneapi-base-toolkit-${pkgver}_offline"/install.sh \
    --silent --eula accept \
    --components all \
    --install-dir "${pkgdir}"/opt/intel/oneapi \
    --log-dir "${srcdir}"/ --ignore-errors

  # allow low level compiler libs to be found
  local _lib_path='/opt/intel/oneapi/compiler'
  local _ldso_conf="${pkgdir}"/etc/ld.so.conf.d
  install -d "${_ldso_conf}"
  echo "${_lib_path}/latest/lib" >> "${_ldso_conf}/${pkgname//-2025}.conf"
  echo "${_lib_path}/latest/opt/compiler/lib" >> "${_ldso_conf}/${pkgname//-2025}.conf"

  # Collection of licenses used in OneAPI with pointers for all toolkits
  install -Dm644 "${pkgdir}/opt/intel/oneapi/licensing/latest/licensing/${_major_ver}.${_minor_ver}/license.htm" \
                 "${pkgdir}/usr/share/licenses/${pkgname//-2025}/license.htm"
}
