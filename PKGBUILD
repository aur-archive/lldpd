# Maintainer: Brian Bidulock <bidulock@openss7.org>

pkgname=lldpd
pkgver=0.7.10
pkgrel=1
pkgdesc="LLDP daemon for GNU/Linux implementing both reception and sending"
arch=('i686' 'x86_64')
url="http://vincentbernat.github.io/lldpd/"
license=('custom:"ISC"')
depends=('libxml2' 'net-snmp' 'libevent' 'libbsd' 'jansson')
options=('!libtool')
install=lldpd.install
backup=('etc/lldpd.conf')
source=("http://media.luffy.cx/files/$pkgname/$pkgname-$pkgver.tar.gz"
	'lldpd.service'
        'lldpd.install'
        'LICENSE')
md5sums=('508f2e76703abf8420d9223aae3db548'
         'b7e8ff9ad84eac551d6975f26b814c49'
         '18d76cccdbbfed66c9c39232dd5f81ae'
         '8ae98663bac55afe5d989919d296f28a')

build() {
  cd "$srcdir/$pkgname-$pkgver"
  ./configure \
    --prefix=/usr \
    --sbindir=/usr/bin \
    --with-snmp \
    --with-xml \
    --with-json \
    --with-privsep-user=lldpd \
    --with-privsep-group=lldpd \
    --with-privsep-chroot=/run/lldpd \
    --with-lldpd-ctl-socket=/run/lldpd.socket \
    --with-lldpd-pid-file=/run/lldpd.pid
  make
  echo "" >>lldpd.conf
  echo "# Place configuration files in this directory: see lldpcli(8)" >README.conf
}

package() {
  cd "$srcdir/$pkgname-$pkgver"
  make DESTDIR="$pkgdir" install
  install -Dm644 lldpd.conf "$pkgdir/etc/lldpd.conf"
  install -Dm644 README.conf "$pkgdir/etc/lldpd.d/README"
  install -Dm644 ../lldpd.service "$pkgdir/usr/lib/systemd/system/lldpd.service"
  install -Dm644 ../LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
