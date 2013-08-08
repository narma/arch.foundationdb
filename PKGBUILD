# Contributor: Sergey Rublev <narma.nsk@gmail.com>

pkgname=foundationdb
pkgver="0.3.0"
pkgrel=1
pkgdesc="Distributed key/value store with ACID transactions"
url="http://www.foundationdb.com/"
license=('Custom foundationdb eula license')
makedepends=('rpmextract')
#depends=()
#optdepends=()
arch=('x86_64')
source=(foundationdb-clients-${pkgver}-${pkgrel}.x86_64.rpm foundationdb-server-${pkgver}-${pkgrel}.x86_64.rpm foundationdb.service)
backup=('etc/foundationdb/foundationdb.conf')
install=foundationdb.install
md5sums=('a4aefc0f9a9a86acdea7889e5f25cf28'
         'abf81aefa330a49efd3b17cb067fc366'
         'e7d685cf975b2035fddd4e98c83a1508')

build() {
  rpmextract.sh *.rpm

  sed -e 's|!/usr/bin/env python|!/usr/bin/env python2|' -i "$srcdir"/usr/lib/$pkgname/backup_agent/fdbbackup
  sed -e 's|!/usr/bin/env python|!/usr/bin/env python2|' -i "$srcdir"/usr/lib/$pkgname/backup_agent/backup_agent
  sed -e 's|!/usr/bin/env python|!/usr/bin/env python2|' -i "$srcdir"/usr/lib/$pkgname/backup_agent/fdbrestore
  sed -e 's|!/usr/bin/env python|!/usr/bin/env python2|' -i "$srcdir"/usr/lib/foundationdb/backup_agent/taskbucket/test.py
  sed -e 's|!/usr/bin/env python|!/usr/bin/env python2|' -i "$srcdir"/usr/lib/foundationdb/make_public.py
  sed -e 's|group = foundationdb|group = daemon|' -i "$srcdir"/etc/foundationdb/foundationdb.conf
}

package() {
  rm -rf "${srcdir}/etc/rc.d"
  cp -R "$srcdir"/{etc,usr} "$pkgdir"/

  install -d -m775 "$pkgdir"/var/lib/foundationdb
  install -d -m775 "$pkgdir"/var/log/foundationdb

  install -Dm644 "$srcdir/foundationdb.service" "$pkgdir/usr/lib/systemd/system/foundationdb.service"
}
