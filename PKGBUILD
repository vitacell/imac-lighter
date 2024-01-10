# Maintainer: harttle <yangjvn@126.com>
# Inspired by macbook-lighter, many thanks to Janhouse's perl script https://github.com/Janhouse/lighter
pkgname=imac-lighter
pkgver=v0.0.2.9.ge9d3b89
pkgrel=1
pkgdesc="iMac screen backlight CLI and auto-adjust on ambient light"
arch=(any)
url="https://github.com/vitacell/imac-lighter" 
license=('GPL')
depends=('bc')
makedepends=('git')
provides=()
conflicts=('macbook-lighter')
source=("git+https://github.com/vitacell/imac-lighter.git")
md5sums=('SKIP')

pkgver() {
  cd "$srcdir/$pkgname"
  git describe --tags | sed 's|-|.|g'
}

package() {
  cd "$srcdir/$pkgname"
  [ ! -f $pkgdir/etc/imac-lighter.conf ] && install -Dm644 imac-lighter.conf $pkgdir/etc/imac-lighter.conf
  install -Dm644 "imac-lighter.service" "$pkgdir/usr/lib/systemd/system/imac-lighter.service"
  install -Dm755 "src/imac-lighter-ambient.sh" "$pkgdir/usr/bin/imac-lighter-ambient"
  install -Dm755 "src/imac-lighter-screen.sh" "$pkgdir/usr/bin/imac-lighter-screen"
}
