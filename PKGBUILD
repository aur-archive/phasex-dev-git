# Contributor: Philipp �berbacher <murks at lavabit dot com>
# Maintainer: David Adler <david dot jo dot adler at gmail dot com>

pkgname=phasex-dev-git
pkgver=20130328
pkgrel=1
pkgdesc="synthesizer using phase offset modulation. 3rd party development version"
arch=('i686' 'x86_64')
url="http://github.com/disabled/phasex-dev"
license=('GPL2')
depends=('lash')
makedepends=('git')
install="$pkgname.install"
provides=('phasex')
conflicts=('phasex')

_gitroot="http://github.com/disabled/phasex-dev.git"
_gitname="phasex-dev"

build() {
  cd "${srcdir}"
  msg "Connecting to GIT server...."

  if [ -d ${_gitname} ] ; then
    cd ${_gitname} && git pull origin
    msg "The local files are updated."
  else
    git clone ${_gitroot}
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting make..."

  rm -rf "${srcdir}/${_gitname}-build"
  git clone "${srcdir}/${_gitname}" "${srcdir}/${_gitname}-build"
  cd "${srcdir}/${_gitname}-build"
  
  # undefined reference to symbol 'g_module_build_path'
  sed -i 's/@PHASEX_LIBS@/& \/usr\/lib\/libgmodule-2.0.so.0/' src/Makefile.am
  # replace obsolete AM_CONFIG_HEADER with AC_CONFIG_HEADERS
  sed -i 's/AM_CONFIG_HEADER/AC_CONFIG_HEADERS/' configure.ac

  aclocal
  autoconf
  automake
  autoheader
 
  ./configure --prefix="/usr" --enable-arch="native"
  make
}
 
package() { 
  cd "${srcdir}/${_gitname}-build"
  make DESTDIR="${pkgdir}/" install

  # desktop file
  install -Dm644 misc/phasex.desktop \
    "$pkgdir/usr/share/applications/phasex.desktop"
  
  # icon
  install -Dm644 pixmaps/phasex-icon.png \
    "$pkgdir/usr/share/pixmaps/phasex-icon.png"
}

# vim:set ts=2 sw=2 et:
