# Maintainer: Jason R. McNeil <jason@jasonrm.net>
# Contributor: Jesus Alvarez <jeezusjr at gmail dot com>
# Contributor: Kyle Fuller <inbox at kylefuller dot co dot uk>

_gitname="spl"

pkgname="spl-utils-git"
pkgver=0.6.3_r76_g6ab0866_3.19.2_1
pkgrel=1
license=('GPL')
pkgdesc="Solaris Porting Layer kernel module support files."
makedepends=("git")
arch=("i686" "x86_64")
url="http://zfsonlinux.org/"
source=("${_gitname}::git+https://github.com/zfsonlinux/spl.git"
        "spl-utils.hostid"
)
groups=("archzfs-git")
sha256sums=('SKIP'
            'ad95131bc0b799c0b1af477fb14fcf26a6a9f76079e48bf090acb7e8367bfd0e')
replaces=("spl-utils")
provides=("spl-utils")
conflicts=("spl-utils" "spl-utils-lts")

pkgver() {
    cd ${srcdir}/${_gitname}
    REPO_VER=$(git describe --long | sed 's/^spl-//;s/\([^-]*-g\)/r\1/;s/-/./g')
    KERNEL_VER=$(pacman -Q linux | awk '{print $2}' | sed -r 's/-/_/g')
    echo "${REPO_VER}_${KERNEL_VER}"
}

build() {
    cd "${srcdir}/${_gitname}"
    ./autogen.sh

    _at_enable=""
    [ "${CARCH}" == "i686"  ] && _at_enable="--enable-atomic-spinlocks"

    ./configure --prefix=/usr \
                --libdir=/usr/lib \
                --sbindir=/usr/bin \
                --with-config=user \
                ${_at_enable}

    make
}

package() {
    cd "${srcdir}/${_gitname}"
    make DESTDIR="${pkgdir}" install

    install -D -m644 "${srcdir}"/spl-utils.hostid "${pkgdir}"/etc/hostid
}
