pkgname=(systemd systemd-libs)
pkgbase=systemd
pkgver=257.8
pkgrel=1
pkgdesc="system and service manager"
arch=('x86_64')
url="https://www.freedesktop.org/wiki/Software/systemd/"
license=('LGPL-2.1-or-later')
groups=('base')
makedepends=(
    'acl'
    'bash'
    'gcc-libs'
    'gcc-libs-32bit'
    'glibc'
    'gperf'
    'intltool'
    'kbd'
    'kmod'
    'libarchive'
    'libcap'
    'libelf'
    'linux-api-headers'
    'lz4'
    'meson'
    'openssl'
    'python-jinja2'
    'shadow'
    'util-linux'
    'xz'
    'zstd'
)
source=(https://github.com/systemd/systemd/archive/v${pkgver}/${pkgbase}-${pkgver}.tar.gz
    30-systemd-binfmt.hook
    30-systemd-catalog.hook
    30-systemd-daemon-reload-system.hook
    30-systemd-daemon-reload-user.hook
    30-systemd-hwdb.hook
    30-systemd-restart-marked.hook
    30-systemd-sysctl.hook
    30-systemd-tmpfiles.hook
    30-systemd-udev-reload.hook
    30-systemd-update.hook
    systemd-hook)
sha256sums=(f280278161446fe3838bedb970c7b3998043ad107f7627735a81483218c6f6f9
    a938701f4244ebdc4ae88ed0032a8315b68f086c6e45d9f2e34ef9442a8f6b47
    be3c749ed51ee26c8412e0304a41ac5e00e668f4d5227b5f359a3b0c31d18d5d
    2d2733a167ee360a36d6cc613001913deb62f199d73e86d39bfe06e669f4b066
    5e35263da327771ec1b9bff1d792b0da4c802f0322e40838f07554788fc191e2
    b19b23467dc33b3e8267cabea10726b0d181e0de1b491ec538b9fb620bccf57f
    6cc362dc73ea4c5094707a603f1810ddfae1a0a9937b89f44d276e2f22cad432
    1af0fbaeaf974fe3d8409854179fac68e8461524dd49849b7e889374363ce3c9
    3cfdc3c21d32cc35b1315f4ff4df87238bc7d2c27bdcf4e5a70600832c481e95
    f6364443609b1d5a07f385e7228ace0eae5040ae3bbd4e00ed5033ef1b19e4b9
    1090b7b1edba2042298b609a77bbe122982ca936208408fb79d77b33a2f3c27a
    aec86b91bb4e335449065fa0dc51dc2b10d0b96a597fdd5f3fdcb1fdc21119e3)

prepare() {
    cd ${pkgbase}-${pkgver}

    sed -e 's/GROUP="render"/GROUP="video"/' \
        -e 's/GROUP="sgx", //'               \
        -i rules.d/50-udev-default.rules.in
}

build() {
    cd ${pkgbase}-${pkgver}

    local _timeservers=(ntp.aliyun.com ntp{1..7}.aliyun.com)

    local meson_args=(
        -D default-dnssec=no
        -D firstboot=false
        -D install-tests=false
        -D ldconfig=false
        -D sysusers=false
        -D rpmmacrosdir=no
        -D homed=disabled
        -D userdb=false
        -D man=disabled
        -D mode=release
        -D pamconfdir=no
        -D dev-kvm-mode=0660
        -D nobody-group=nogroup
        -D sysupdate=disabled
        -D ukify=disabled
        -D docdir=/usr/share/doc/${pkgname}-${pkgver}
        -D ntp-servers="${_timeservers[*]}"
        -D fallback-hostname='flarebird'
        -D sbat-distro='Flarebird'
        -D sbat-distro-summary="Flarebird"
        -D sbat-distro-pkgname=${pkgname}
        -D sbat-distro-version=${pkgver}

    )

    ${meson_options} "${meson_args[@]}"

    ${meson_build}
}

package_systemd() {
    pkgdesc="system and service manager"
    license+=(
        'CC0-1.0'
        'GPL-2.0-or-later'
        'MIT-0'
    )
    depends=(
        'acl'
        'bash'
        'gcc-libs'
        'glibc'
        'kbd'
        'kmod'
        'libcap'
        'libelf'
        'lz4'
        'openssl'
        "systemd-libs=${pkgver}"
        'util-linux-libs'
        'xz'
        'zstd'
    )
    backup=(
        etc/systemd/coredump.conf
        etc/systemd/journald.conf
        etc/systemd/journal-upload.conf
        etc/systemd/logind.conf
        etc/systemd/networkd.conf
        etc/systemd/oomd.conf
        etc/systemd/pstore.conf
        etc/systemd/resolved.conf
        etc/systemd/sleep.conf
        etc/systemd/system.conf
        etc/systemd/timesyncd.conf
        etc/systemd/user.conf
        etc/udev/iocost.conf
        etc/udev/udev.conf
    )
    install=${pkgname}.install

    cd ${pkgbase}-${pkgver}

    ${meson_install} ${pkgdir}

    chown 0:23 ${pkgdir}/var/log/journal
    chmod 2755 ${pkgdir}/var/log/journal

    rm -rf ${pkgdir}/var/log/journal/remote

    # pacman hooks
    install -vDm755 ${srcdir}/systemd-hook ${pkgdir}/usr/share/libalpm/scripts/systemd-hook
    install -vDm644 ${srcdir}/*.hook -t ${pkgdir}/usr/share/libalpm/hooks

    _pick systemd-libs ${pkgdir}/usr/lib64/lib{nss,systemd,udev}*.so*
    _pick systemd-libs ${pkgdir}/usr/lib64/pkgconfig
    _pick systemd-libs ${pkgdir}/usr/include

}

package_systemd-libs() {
    pkgdesc="systemd client libraries"
    depends=('glibc' 'gcc-libs' 'libcap' 'lz4' 'xz' 'zstd')
    license+=(
        'CC0-1.0'
        'GPL-2.0-or-later WITH Linux-syscall-note'
    )

    mv ${pkgname}/* ${pkgdir}
}
