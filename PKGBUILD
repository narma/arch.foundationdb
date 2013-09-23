# Contributor: Sergey Rublev <narma.nsk@gmail.com>

pkgname=foundationdb
pkgver="1.0.1"
pkgrel=1
pkgdesc="Distributed key/value store with ACID transactions"
url="https://foundationdb.com/downloads/I_accept_the_FoundationDB_Community_License_Agreement"
license=('Custom foundationdb eula license')
makedepends=('libarchive')
arch=('x86_64')
source=(${url}/${pkgver}/foundationdb-clients_${pkgver}-${pkgrel}_amd64.deb
        ${url}/${pkgver}/foundationdb-server_${pkgver}-${pkgrel}_amd64.deb
        foundationdb.service)
backup=('etc/foundationdb/foundationdb.conf')
install=foundationdb.install
md5sums=('d679291e4d6c3565fa176e88cc955f00'
         '9349065115c66b9ecf0571e1c033ae63'
         'e7d685cf975b2035fddd4e98c83a1508')

extract_deb() {
  bsdtar -xf $1 data.tar.gz
  bsdtar -xf data.tar.gz
  rm data.tar.gz
}

build() {

  extract_deb "foundationdb-clients_${pkgver}-${pkgrel}_amd64.deb"
  extract_deb "foundationdb-server_${pkgver}-${pkgrel}_amd64.deb"

  # It's debian specific
  mkdir "$srcdir/usr/lib/python2.7/site-packages/"
  mv "$srcdir/usr/lib/python2.7/dist-packages/fdb" "$srcdir/usr/lib/python2.7/site-packages/"
  
  sed -e 's|!/usr/bin/python|!/usr/bin/env python2|' -i "$srcdir"/usr/lib/$pkgname/backup_agent/fdbbackup
  sed -e 's|!/usr/bin/python|!/usr/bin/env python2|' -i "$srcdir"/usr/lib/$pkgname/backup_agent/backup_agent
  sed -e 's|!/usr/bin/python|!/usr/bin/env python2|' -i "$srcdir"/usr/lib/$pkgname/backup_agent/fdbrestore
  sed -e 's|!/usr/bin/python|!/usr/bin/env python2|' -i "$srcdir"/usr/lib/foundationdb/backup_agent/taskbucket/test.py
  sed -e 's|!/usr/bin/python|!/usr/bin/env python2|' -i "$srcdir"/usr/lib/foundationdb/make_public.py
  sed -e 's|group = foundationdb|group = daemon|' -i "$srcdir"/etc/foundationdb/foundationdb.conf

  rm -rf "$srcdir/etc/init.d"
  rmdir "$srcdir/usr/lib/python2.7/dist-packages/"
}

package() {
  cp -R "$srcdir"/{etc,usr} "$pkgdir"/

  install -d -m775 "$pkgdir"/var/lib/foundationdb
  install -d -m775 "$pkgdir"/var/log/foundationdb

  install -Dm644 "$srcdir/foundationdb.service" "$pkgdir/usr/lib/systemd/system/foundationdb.service"
}
