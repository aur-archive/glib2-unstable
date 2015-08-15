pkgbase=glib2-unstable
pkgname=(glib2-unstable glib2-unstable-docs)
pkgver=2.43.0
pkgrel=1
pkgdesc="Common C routines used by GTK+ and other libs [Unstable version]"
url="http://www.gtk.org/"
arch=(i686 x86_64)
provides=('glib2' 'glib2-docs')
makedepends=('pkg-config' 'python2' 'libxslt' 'docbook-xml' 'pcre' 'libffi' 'elfutils')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/${pkgver:0:4}/glib-$pkgver.tar.xz)
md5sums=('c93f4364a2bc82807a5ea7ae220a9e35')


build() {
  cd glib-$pkgver
  PYTHON=/usr/bin/python2 ./configure --prefix=/usr      \
                                      --libdir=/usr/lib  \
                                      --sysconfdir=/etc  \
                                      --with-pcre=system \
                                      --disable-fam
  make
}

package_glib2-unstable() {
  conflicts=('glib2')
  provides=('glib2')
  depends=('pcre' 'libffi')
  optdepends=('python2: for gdbus-codegen and gtester-report'
              'elfutils: gresource inspection tool')
  options=('!docs' '!emptydirs')
  license=('LGPL')

  cd glib-$pkgver
  make completiondir=/usr/share/bash-completion/completions DESTDIR="$pkgdir" install

  for _i in "$pkgdir/usr/share/bash-completion/completions/"*; do
      chmod -x "$_i"
  done

  # Our gdb does not ship the required python modules, so remove it
  rm -rf "$pkgdir/usr/share/gdb/"
}

package_glib2-unstable-docs() {
  pkgdesc="Documentation for glib2"
  conflicts=('gobject2-docs' 'glib2-docs')
  replaces=('gobject2-docs')
  provides=('glib2-docs')
  license=('custom')
  options=('docs' '!emptydirs')

  cd glib-$pkgver/docs
  make DESTDIR="${pkgdir}" install
  install -m755 -d "${pkgdir}/usr/share/licenses/glib2-docs"
  install -m644 reference/COPYING "${pkgdir}/usr/share/licenses/glib2-docs/"

  rm -rf "${pkgdir}/usr/share/man"
}
