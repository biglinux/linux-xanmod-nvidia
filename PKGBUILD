# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Contributor: Thomas Baechler <thomas@archlinux.org>

_linuxprefix=linux-xanmod

pkgname="${_linuxprefix}-nvidia"
pkgdesc="NVIDIA drivers for linux"
pkgver=550.100
pkgrel=69121
arch=('x86_64')
url="http://www.nvidia.com/"
license=('custom')
groups=("${_linuxprefix}-extramodules")
depends=("${_linuxprefix}" "nvidia-utils=${pkgver}")
makedepends=("${_linuxprefix}-headers")
provides=("nvidia=${pkgver}" 'NVIDIA-MODULE')
options=(!strip)
_durl="https://us.download.nvidia.com/XFree86/Linux-x86"
source=("${_durl}_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run")
sha256sums=(SKIP)

_pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
    sh "${_pkg}.run" --extract-only
}

build() {
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"
    _kernver="$(cat /usr/src/${_linuxprefix}/version)"

    cd "${_pkg}"
    make -C kernel SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
    _kernver="$(cat /usr/src/${_linuxprefix}/version)"

    cd "${_pkg}"
    install -Dm 644 kernel/*.ko -t "${pkgdir}/usr/lib/modules/${_kernver}/extramodules/"

    # compress each module individually
    find "${pkgdir}" -name '*.ko' -exec xz -T1 {} +

    install -Dm 644 LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}/"
}
