# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Contributor: Thomas Baechler <thomas@archlinux.org>

_linuxprefix=linux-xanmod

pkgname="${_linuxprefix}-nvidia"
pkgdesc="NVIDIA drivers for linux"
pkgver=550.90.07
pkgrel=69310
arch=('x86_64')
url="http://www.nvidia.com/"
license=('custom')
groups=("${_linuxprefix}-extramodules")
depends=("${_linuxprefix}" "nvidia-utils=${pkgver}")
makedepends=("${_linuxprefix}-headers")
provides=("nvidia=${pkgver}" 'NVIDIA-MODULE')
options=(!strip)
_durl="https://us.download.nvidia.com/XFree86/Linux-x86"
source=("${_durl}_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run"
        '0001-NVIDIA-take-modeset-ownership-early.patch'
        'make-modeset-fbdev-default.patch')
sha256sums=('5a92545013649d6435d46fc8b5af617f4e3fdea78ee435e034c14bc47557c117'
            '5dde67ef74cc5ca6e96e9984886d58d17087794e81209f7154e450d670195250'
            'd14d6c182446a090a043ae8547f98df2dc8660d3a1bcd98654406ad99ae0ad6c')

_pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
    sh "${_pkg}.run" --extract-only

    cd "${_pkg}"
#    local src
#    for src in "${source[@]}"; do
#        src="${src%%::*}"
#        src="${src##*/}"
#        [[ $src = *.patch ]] || continue
#        msg2 "Applying patch: $src..."
#        patch -Np1 < "../$src"
#    done

    patch -Np1 -i "$srcdir"/0001-NVIDIA-take-modeset-ownership-early.patch

    cd kernel
    patch -Np1 -i "$srcdir"/make-modeset-fbdev-default.patch
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
