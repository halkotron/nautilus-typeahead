# $Id: PKGBUILD 278826 2016-10-15 00:15:40Z heftig $
# Contributor: Jan de Groot <jgc@archlinux.org>
# Contributor: Ian Hernández <badwolfie@archlinux.info>
# Maintainer: Albert Vaca Cintora <albertvaka@gmail.com>

_pkgbase=nautilus
pkgbase=nautilus-typeahead
pkgname=(nautilus-typeahead libnautilus-extension-typeahead)
pkgver=3.32.0
pkgrel=2
pkgdesc="Default file manager for GNOME - Patched to bring back the 'typeahead find' feature"
url="https://wiki.gnome.org/Apps/Nautilus"
arch=(i686 x86_64)
license=(GPL)
depends=(libgexiv2 gnome-desktop gvfs dconf tracker gnome-autoar gst-plugins-base-libs tracker-miners)
makedepends=(gobject-introspection packagekit git gtk-doc meson appstream-glib 'meson>=0.44.1' ninja)
optdepends=('nautilus-sendto: right click to send files')
options=(!emptydirs)
_commit=6114cb85855406a8cb0f339f5b0cfffca5789d95
source=("git+https://gitlab.gnome.org/GNOME/nautilus.git#commit=$_commit"
        nautilus-restore-typeahead.patch)
sha256sums=('SKIP'
            'd3e1a3df1042a412aa23d0a1710c490c117239cd4d9ae9bae2ee32e190c8c03f')

prepare() {
  cd $_pkgbase

  git submodule init
  git submodule update

  patch -p1 -i ../nautilus-restore-typeahead.patch
}

pkgver() {
  cd $_pkgbase
  git describe --tags | sed 's/-/+/g'
}

build() {
  arch-meson $_pkgbase build -D docs=true
  ninja -C build
}

package_nautilus-typeahead() {
  depends+=(libnautilus-extension-typeahead)
  conflicts=(nautilus)
  provides=(nautilus)
  groups=(gnome)

  DESTDIR="$pkgdir" ninja -C build install

### Split libnautilus-extension
  mkdir -p libne/{lib,share}
  mv "$pkgdir"/usr/include libne
  mv "$pkgdir"/usr/lib/{girepository-1.0,libnautilus-extension*,pkgconfig} libne/lib
  mv "$pkgdir"/usr/share/{gir-1.0,gtk-doc} libne/share
}

package_libnautilus-extension-typeahead() {
  pkgdesc="Library for extending the $pkgdesc"
  conflicts=(libnautilus-extension)
  provides=(libnautilus-extension)
  depends=(gtk3)

  mv libne "$pkgdir/usr"
}
