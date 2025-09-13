pkgname=(systemd systemd-libs)
pkgbase=systemd
pkgver=257.9
pkgrel=2
pkgdesc="system and service manager"
arch=('x86_64')
url="https://www.freedesktop.org/wiki/Software/systemd/"
license=('LGPL-2.1-or-later')
makedepends=(
    'acl'
    'audit'
    'bash'
    'bash-completion'
    'bpf'
    'clang'
    'cryptsetup'
    'curl'
    'dbus'
    'docbook-xml'
    'docbook-xsl'
    'git'
    'gnutls'
    'gperf'
    'hwdata'
    'intltool'
    'kbd'
    'kmod'
    'gcc-libs-32bit'
    'kexec-tools'
    'libarchive'
    'libbpf'
    'libcap'
    'libelf'
    'libfido2'
    'libgcrypt'
    'libidn2'
    'libmicrohttpd'
    'libpwquality'
    'libqrencode'
    'libseccomp'
    'libxcrypt'
    'libxslt'
    'linux-api-headers'
    'linux-pam'
    'llvm'
    'lz4'
    'meson'
    'openssl'
    'p11-kit'
    'pcre2'
    'python-jinja2'
    'python-lxml'
    'python-pefile'
    'python-pyelftools'
    'quota-tools'
    'rsync'
    'shadow'
    'systemd'
    'util-linux'
    'xz'
    'zstd'
)
source=(https://github.com/systemd/systemd/archive/v${pkgver}/${pkgbase}-${pkgver}.tar.gz
    20-systemd-sysusers.hook
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
    systemd-hook
    systemd-user.pam)
sha256sums=('b27dcc100a738b4b5b81f7c5174c1239a6495f5bdf3d3caa94a17b8373e6a1ca'
    4e30440b394f4669a33f9b8d49ae67966325f6662bbc03a98b325a2b186adf7c
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
    8a4460981cabcc85054ee61881d98ad9168f7af0fc066c9adef04392a9c4b077
    d1cf9c0827615d7d257c135bc8d9b83ccb72543805fef309fc808946f895d1df)

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
        -D version-tag="${pkgver}-${pkgrel}-flarebird"
        -D default-dnssec=no
        -D firstboot=false
        -D install-tests=false
        -D man=enabled
        -D ldconfig=false
        -D sysusers=true
        -D rpmmacrosdir=no
        -D homed=enabled
        -D userdb=true
        -D mode=release
        -D pam=enabled
        -D pamconfdir=/etc/pam.d
        -D dev-kvm-mode=0660
        -D nobody-group=nogroup
        -D ukify=enabled
        -D docdir=/usr/share/doc/${pkgbase}-${pkgver}
        -D ntp-servers="${_timeservers[*]}"
        -D fallback-hostname='flarebird'

        -D libcurl=enabled
        -D libidn2=enabled
        -D openssl=enabled
        -D cryptolib=openssl
        -D p11kit=enabled
        -D pwquality=enabled
        -D seccomp=enabled
        -D efi=true
        -D lz4=enabled
        -D selinux=disabled
        -D apparmor=disabled
        -D bootloader=enabled
        -D xenctrl=disabled
        -D bpf-framework=enabled
        -D ima=false

        -D resolve=true
        -D timesyncd=true
        -D microhttpd=enabled
        -D libfido2=enabled

        -D dns-over-tls=openssl
        -D sysvinit-path=
        -D sysvrcnd-path=

        -D sbat-distro='Flarebird'
        -D sbat-distro-summary="Flarebird Linux"
        -D sbat-distro-pkgname=${pkgbase}
        -D sbat-distro-version=${pkgver}
        -D sbat-distro-url="https://FlarebirdOS.github.io"
    )

    ${meson_options} "${meson_args[@]}" ${MESON_EXTRA_CONFIGURE_OPTIONS}

    ${meson_build}
}

package_systemd() {
    pkgdesc="system and service manager"
    license+=(
        'CC0-1.0'
        'GPL-2.0-or-later'
        'MIT-0'
    )
    groups=('base')
    depends=(
        'acl'
        'audit'
        'bash'
        'cryptsetup'
        'dbus'
        'hwdata'
        'kbd'
        'kmod'
        'libcap'
        'libelf'
        'libgcrypt'
        'libidn2'
        'libseccomp'
        'libxcrypt'
        'linux-pam'
        'lz4'
        'openssl'
        'pcre2'
        "systemd-libs=${pkgver}"
        'util-linux'
        'xz'
    )
    backup=(
        etc/systemd/coredump.conf
        etc/systemd/homed.conf
        etc/systemd/journald.conf
        etc/systemd/journal-remote.conf
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

    install -vDm644 ${srcdir}/systemd-user.pam ${pkgdir}/etc/pam.d/systemd-user

    chown 0:23 ${pkgdir}/var/log/journal
    chmod 2755 ${pkgdir}/var/log/journal

    rm -rf ${pkgdir}/var/log/journal/remote

    # pacman hooks
    install -vDm755 ${srcdir}/systemd-hook ${pkgdir}/usr/share/libalpm/scripts/systemd-hook
    install -vDm644 ${srcdir}/*.hook -t ${pkgdir}/usr/share/libalpm/hooks

    # create a directory for cryptsetup keys
    install -d -m0700 ${pkgdir}/etc/cryptsetup-keys.d

    sed -e "s/^g render/# g render/g" \
        -e "s/g sgx/# g sgx/g"        \
        -i ${pkgdir}//usr/lib/sysusers.d/basic.conf

    _pick systemd-libs ${pkgdir}/usr/lib64/lib{nss,systemd,udev}*.so*
    _pick systemd-libs ${pkgdir}/usr/lib64/pkgconfig
    _pick systemd-libs ${pkgdir}/usr/include
    _pick systemd-libs ${pkgdir}/usr/share/man/man3
}

package_systemd-libs() {
    pkgdesc="systemd client libraries"
    depends=(
        'gcc-libs'
        'glibc'
        'libcap'
        'libgcrypt'
        'lz4'
        'xz'
        'zstd'
    )
    license+=(
        'CC0-1.0'
        'GPL-2.0-or-later WITH Linux-syscall-note'
    )

    mv ${pkgname}/* ${pkgdir}
}
