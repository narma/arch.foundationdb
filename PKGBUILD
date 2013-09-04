# Contributor: Sergey Rublev <narma.nsk@gmail.com>

pkgname=foundationdb
pkgver="1.0.0"
pkgrel=1
pkgdesc="Distributed key/value store with ACID transactions"
url="https://foundationdb.com/downloads/I_accept_the_FoundationDB_Community_License_Agreement"
license=('Custom foundationdb eula license')
makedepends=('rpmextract')
#depends=()
#optdepends=()
arch=('x86_64')
source=(${url}/foundationdb-clients-${pkgver}-${pkgrel}.x86_64.rpm
        ${url}/foundationdb-server-${pkgver}-${pkgrel}.x86_64.rpm
        foundationdb.service)
backup=('etc/foundationdb/foundationdb.conf')
install=foundationdb.install
md5sums=('7e573a21d53408e12cc236aded112812'
         '1117bdca0e2b5766bf17b5440bdd00f5'
         'e7d685cf975b2035fddd4e98c83a1508')

build() {
  rpmextract.sh *.rpm

  sed -e 's|!/usr/bin/python|!/usr/bin/env python2|' -i "$srcdir"/usr/lib/$pkgname/backup_agent/fdbbackup
  sed -e 's|!/usr/bin/python|!/usr/bin/env python2|' -i "$srcdir"/usr/lib/$pkgname/backup_agent/backup_agent
  sed -e 's|!/usr/bin/python|!/usr/bin/env python2|' -i "$srcdir"/usr/lib/$pkgname/backup_agent/fdbrestore
  sed -e 's|!/usr/bin/python|!/usr/bin/env python2|' -i "$srcdir"/usr/lib/foundationdb/backup_agent/taskbucket/test.py
  sed -e 's|!/usr/bin/python|!/usr/bin/env python2|' -i "$srcdir"/usr/lib/foundationdb/make_public.py
  sed -e 's|group = foundationdb|group = daemon|' -i "$srcdir"/etc/foundationdb/foundationdb.conf
  
  # remove python2.6 binding if exist
  if [ -d "$srcdir"/usr/lib64/python2.6 ]; then
    rm -rf "$srcdir"/usr/lib64/python2.6
  fi
}

package() {
  rm -rf "${srcdir}/etc/rc.d"
  cp -R "$srcdir"/{etc,usr} "$pkgdir"/

  install -d -m775 "$pkgdir"/var/lib/foundationdb
  install -d -m775 "$pkgdir"/var/log/foundationdb

  install -Dm644 "$srcdir/foundationdb.service" "$pkgdir/usr/lib/systemd/system/foundationdb.service"
}
