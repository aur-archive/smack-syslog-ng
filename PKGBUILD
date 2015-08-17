# $Id: PKGBUILD 155735 2012-04-06 00:04:14Z tomegun $
# Maintainer: Dave Reisner <dreisner@archlinux.org>
# Contributor: Eric Bélanger <eric@archlinux.org>
# Contributor: Aaron Griffin <aaron@archlinux.org>
# Contributor: Wolfgang Bumiller <fc2.oyho@fcrrq.ng> (rot13 encoded)

pkgname=smack-syslog-ng
_pkgname=syslog-ng
pkgver=3.3.4
pkgrel=5
pkgdesc="Next-generation syslogd with advanced networking and filtering capabilities. Modified to run under a label other than floor '_'"
arch=('i686' 'x86_64')
license=('GPL2')
groups=('base')
url="http://www.balabit.com/network-security/syslog-ng/"
depends=('glib2' 'eventlog' 'openssl' 'libcap' 'awk' 'libwbsmack')
makedepends=('flex' 'pkg-config')
optdepends=('logrotate: for rotating log files')
provides=('logger' "$_pkgname-$pkgver-$pkgrel")
conflicts=("$_pkgname")
options=('!libtool')
install=install
backup=('etc/syslog-ng/modules.conf'
        'etc/syslog-ng/scl.conf'
        'etc/syslog-ng/syslog-ng.conf'
        'etc/conf.d/syslog-ng'
        'etc/logrotate.d/syslog-ng')
source=("http://www.balabit.com/downloads/files/syslog-ng/sources/$pkgver/source/${_pkgname}_$pkgver.tar.gz"
        syslog-ng.conf
        syslog-ng.conf.d
        syslog-ng.logrotate
        syslog-ng.rc
        syslog-smack.patch)
sha1sums=('3437a50af027f281747087ab47a45aa5fbabbf14'
          '98074e0facfc6ef036202662cc86d04b38a2c142'
          '9b2eb6ea9e27c9f1b6c1c855be211ec3da51d3c8'
          '949128fe3d7f77a7aab99048061f885bc758000c'
          '94af81a84e3add6653755122cdd5080694de059d'
          '0bdd3c64694cbb10ea6afc32224d975b93f03eae')

build() {
  cd "$_pkgname-$pkgver"

  patch -Np1 -i "$srcdir/syslog-smack.patch"

  ./configure \
    --prefix=/usr \
    --sysconfdir=/etc/syslog-ng \
    --libexecdir=/usr/lib \
    --localstatedir=/var/lib/syslog-ng \
    --datadir=/usr/share/syslog-ng \
    --with-pidfile-dir=/run \
    --disable-spoof-source \
    --enable-systemd \
    --with-systemdsystemunitdir=/usr/lib/systemd/system

  make
}

package() {
  make -C "$_pkgname-$pkgver" DESTDIR="$pkgdir" install

  install -dm755 "$pkgdir/var/lib/syslog-ng" "$pkgdir/etc/syslog-ng/patterndb.d"
  install -Dm644 "$srcdir/syslog-ng.conf" "$pkgdir/etc/syslog-ng/syslog-ng.conf"
  install -Dm644 "$srcdir/syslog-ng.logrotate" "$pkgdir/etc/logrotate.d/syslog-ng"
  install -Dm755 "$srcdir/syslog-ng.rc" "$pkgdir/etc/rc.d/syslog-ng"
  install -Dm644 "$srcdir/syslog-ng.conf.d" "$pkgdir/etc/conf.d/syslog-ng"

  # fix location of systemctl
  sed -i 's@/bin/systemctl@/usr&@' "$pkgdir/usr/lib/systemd/system/syslog-ng.service"
}
