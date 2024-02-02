# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Archlinux maintainer : Thomas Baechler <thomas@archlinux.org>

_linuxprefix=linux-xanmod
_kernver=$(find /usr/lib/modules -type d -iname 6.6.15*xanmod* | rev | cut -d "/" -f1 | rev)

pkgname=$_linuxprefix-nvidia
pkgdesc="NVIDIA drivers for linux"
pkgver=545.29.06
pkgrel=66151
arch=('x86_64')
url="http://www.nvidia.com/"
license=('custom')
groups=("$_linuxprefix-extramodules")
depends=("$_linuxprefix" "nvidia-utils=$pkgver")
makedepends=("$_linuxprefix-headers")
provides=("nvidia=$pkgver" 'NVIDIA-MODULE')
options=(!strip)
_durl="https://us.download.nvidia.com/XFree86/Linux-x86"
source=("${_durl}_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run" 'GPL-workaround.patch')
sha256sums=('22ce8f5f617ebf13f75510fc4f47ae307b067cc464ed59852631ba3cf149f26d'
            '59958c134261a53edb641ba3c96b13e397d1903ec3637c8be8d61141356292de')

_pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
    sh "${_pkg}.run" --extract-only

    cd "${_pkg}"
    patch -p1 -i ../GPL-workaround.patch
}

build() {

    cd "${_pkg}"
    make -C kernel SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
    cd "${_pkg}"
    install -Dm 644 kernel/*.ko -t "${pkgdir}/usr/lib/modules/${_kernver}/extramodules/"

    # compress each module individually
    find "${pkgdir}" -name '*.ko' -exec xz -T1 {} +

    install -Dm 644 LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}/"
}
