# $Id$
# Maintainer of original package: Thomas Baechler <thomas@archlinux.org>
# Contributor to patched package: Bernard Baeyens (berbae) <berbae52 at sfr dot fr>
# Security Vulnerability patch for: CVE-2012-0946

pkgname=nvidia-patched
pkgver=295.33
_extramodules=extramodules-3.3-ARCH
_kernver="$(cat /lib/modules/${_extramodules}/version)"
pkgrel=3
pkgdesc="NVIDIA drivers for linux."
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
depends=('linux>=3.3' 'linux<3.4' "nvidia-utils-patched=${pkgver}")
makedepends=('linux-headers>=3.3' 'linux-headers<3.4')
conflicts=('nvidia' 'nvidia-96xx' 'nvidia-173xx')
license=('custom')
install=nvidia.install
options=(!strip)

if [ "$CARCH" = "i686" ]; then
    _arch='x86'
    _pkg="NVIDIA-Linux-${_arch}-${pkgver}"
    source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run"
	    "ftp://download.nvidia.com/XFree86/patches/security/CVE-2012-0946/nvidia-blacklist-register-mapping-290-295.diff")
    md5sums=('1634fb3115526caeae5eb8227282bf17'
	    'aff18975d955e0a6f929a3b72d2c2202')
elif [ "$CARCH" = "x86_64" ]; then
    _arch='x86_64'
    _pkg="NVIDIA-Linux-${_arch}-${pkgver}-no-compat32"
    source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run"
	    "ftp://download.nvidia.com/XFree86/patches/security/CVE-2012-0946/nvidia-blacklist-register-mapping-290-295.diff")
    md5sums=('04e814843bcaf23efa0b4b3df4ea2700'
	    'aff18975d955e0a6f929a3b72d2c2202')
fi

build() {
    cd "${srcdir}"
    sh "${_pkg}.run" --apply-patch nvidia-blacklist-register-mapping-290-295.diff
    sh "${_pkg}-custom.run" --extract-only
    cd "${_pkg}-custom/kernel"
    make SYSSRC=/lib/modules/"${_kernver}/build" module
}

package() {
    install -D -m644 "${srcdir}/${_pkg}-custom/kernel/nvidia.ko" \
        "${pkgdir}/lib/modules/${_extramodules}/nvidia.ko"
    install -d -m755 "${pkgdir}/lib/modprobe.d"
    echo "blacklist nouveau" >> "${pkgdir}/lib/modprobe.d/nvidia.conf"
    sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" "${startdir}/nvidia.install"
    gzip "${pkgdir}/lib/modules/${_extramodules}/nvidia.ko"
}
