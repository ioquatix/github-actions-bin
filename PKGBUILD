# Maintainer  : Samuel Williams <samuel@oriontransfer.net>
# Contributor : Edvinas Valatka <edacval@gmail.com>
# Contributor : Jingbei Li <i@jingbei.li>

_pkgname=github-actions
pkgname=${_pkgname}-bin
pkgver=2.273.4
pkgrel=1
pkgdesc='GitHub Actions self-hosted runner tools.'
arch=('x86_64' 'armv6h' 'armv7h' 'aarch64')
url='https://github.com/actions/runner'
license=('MIT')

OPTIONS=(!strip !docs libtool emptydirs)

install=PKGBUILD

provides=($_pkgname)
conflicts=($_pkgname)
_common_source=(github-actions.service github-actions.tmpfiles github-actions.sysusers)
source=(
       "https://github.com/actions/runner/releases/download/v$pkgver/actions-runner-linux-x64-$pkgver.tar.gz"
       ${_common_source[@]}
)
source_armv6h=(
       "https://github.com/actions/runner/releases/download/v$pkgver/actions-runner-linux-arm-$pkgver.tar.gz"
       ${_common_source[@]}
)
source_armv7h=(${source_armv6h[@]})
source_aarch64=(
       "https://github.com/actions/runner/releases/download/v$pkgver/actions-runner-linux-arm64-$pkgver.tar.gz"
       ${_common_source[@]}
)

sha512sums=('f7cd6538727168c8a9bb9bdd8b951b9ca3dcfb82653ab02d290772958d15779a60465764f655079ee3c9fd113cb1ad436fed445e97f7c929315e7dad325a299c'
            'abeb32b58cd526bb6abe928d978664de85cab5715d93189919412f889a8a1089a11ec1e8bf21e72e96e8640ca85ecb97daff0c797541317dbe4f6f508c39858d'
            'c0a8cc36b353f85d9e05868a20b160c0cd36d983bcb65562dd8c3f2c6321061deec18331156ec75f0b9d7b0009df7412abf2d977e6995cac004c5462282928a8'
            '49329a3c65987f7bb219100b41deb33fcbc64f5e6424c4e31d580e2fbd408545d2d4a990c5511a3625250bd37ad7d13496cfd152ffd20de04fd24250242088d4')
sha512sums_armv6h=('a77f15f741b514ad0891b9efe6d1ba1abf344a8010a66ed3ba9a876ba6c15653916319afee99dceae501a634e655b3bd18d3bceb49b14269212ef7d8ece7fda8'
                   'abeb32b58cd526bb6abe928d978664de85cab5715d93189919412f889a8a1089a11ec1e8bf21e72e96e8640ca85ecb97daff0c797541317dbe4f6f508c39858d'
                   'c0a8cc36b353f85d9e05868a20b160c0cd36d983bcb65562dd8c3f2c6321061deec18331156ec75f0b9d7b0009df7412abf2d977e6995cac004c5462282928a8'
                   '49329a3c65987f7bb219100b41deb33fcbc64f5e6424c4e31d580e2fbd408545d2d4a990c5511a3625250bd37ad7d13496cfd152ffd20de04fd24250242088d4')
sha512sums_armv7h=('a77f15f741b514ad0891b9efe6d1ba1abf344a8010a66ed3ba9a876ba6c15653916319afee99dceae501a634e655b3bd18d3bceb49b14269212ef7d8ece7fda8'
                   'abeb32b58cd526bb6abe928d978664de85cab5715d93189919412f889a8a1089a11ec1e8bf21e72e96e8640ca85ecb97daff0c797541317dbe4f6f508c39858d'
                   'c0a8cc36b353f85d9e05868a20b160c0cd36d983bcb65562dd8c3f2c6321061deec18331156ec75f0b9d7b0009df7412abf2d977e6995cac004c5462282928a8'
                   '49329a3c65987f7bb219100b41deb33fcbc64f5e6424c4e31d580e2fbd408545d2d4a990c5511a3625250bd37ad7d13496cfd152ffd20de04fd24250242088d4')
sha512sums_aarch64=('cb8cbd24da26b84b7e17320c6c9a6411037627ea7ea23d112cdb51d9b9daa557ec5afc2ecf6d7466d5046c9379a79ea9b73144da73300940f3d669d5f8845c23'
                    'abeb32b58cd526bb6abe928d978664de85cab5715d93189919412f889a8a1089a11ec1e8bf21e72e96e8640ca85ecb97daff0c797541317dbe4f6f508c39858d'
                    'c0a8cc36b353f85d9e05868a20b160c0cd36d983bcb65562dd8c3f2c6321061deec18331156ec75f0b9d7b0009df7412abf2d977e6995cac004c5462282928a8'
                    '49329a3c65987f7bb219100b41deb33fcbc64f5e6424c4e31d580e2fbd408545d2d4a990c5511a3625250bd37ad7d13496cfd152ffd20de04fd24250242088d4')

package() {
       depends=(sudo)
       mkdir -p "$pkgdir"/var/lib/$_pkgname

       # Useless on pacman-based distributions
       rm -f "$srcdir"/bin/installdependencies.sh

       cp -r -t "$pkgdir"/var/lib/$_pkgname "$srcdir"/{bin,externals,*.sh}

       # also see github-actions.tmpfiles
       chmod 0775 "$pkgdir"/var/lib/$_pkgname

       # make ldd happy
       chmod +x "$pkgdir"/var/lib/$_pkgname/bin/*.so

       install -Dm644 "$srcdir"/$_pkgname.tmpfiles "${pkgdir}"/usr/lib/tmpfiles.d/$_pkgname.conf
       install -Dm644 "$srcdir"/$_pkgname.sysusers "${pkgdir}"/usr/lib/sysusers.d/$_pkgname.conf
       install -Dm644 "$srcdir"/$_pkgname.service  "${pkgdir}"/usr/lib/systemd/system/$_pkgname.service
}

pre_remove() {
       if systemctl -q is-enabled $_pkgname.service; then
              systemctl disable $_pkgname.service
       fi
}

post_remove() {
       echo
       echo "Remove $_pkgname user and this HOME /var/lib/$_pkgname manually, if not needed anymore."
       echo
}

