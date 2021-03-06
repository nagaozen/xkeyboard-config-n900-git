# $Id$
# Maintainer: Joni Lapilainen <joni.lapilainen@gmail.com>
# Contributor: Alexander Baldeck <kth5@archlinux.org>
# Contributor: Sébastien Mazy <melyadon@gmail.com>
# Contributor: Gilrain <pierre.buard+aur gmail com>
pkgname=xkeyboard-config-n900-git
pkgver=20130225
pkgrel=1
pkgdesc="X keyboard configuration files with extended N900 keys (git version)"
arch=(any)
license=('custom')
url="http://www.freedesktop.org/wiki/Software/XKeyboardConfig"
depends=('xorg-xkbcomp')
makedepends=('git' 'intltool' 'xorg-util-macros')
provides=('xkbdata' 'xkeyboard-config')
replaces=('xkbdata')
conflicts=('xkbdata' 'xkeyboard-config')

source=(
'0001-RX-51-Symbols-Bind-Escape-to-third-level-Backspace.patch'
'0002-RX-51-Symbols-Bind-function-keys-to-fourth-level-top.patch'
'0003-RX-51-Symbols-Bind-PgUp-PgDown-Home-End-to-third-lev.patch'
'0004-RX-51-Symbols-Bind-less-and-greater-to-fourth-level-.patch'
'0005-RX-51-Symbols-Bind-volume-keys-as-XF86-raise-and-low.patch'
'0006-RX-51-Symbols-Bind-bar-to-fourth-level-L.patch'
'0007-RX-51-Symbols-Bind-tilde-to-fourth-level-C-and-F.patch'
'0008-RX-51-Symbols-Bind-percent-to-fourth-level-D.patch'
'0009-RX-51-Symbols-Bind-Tab-to-third-level-Return.patch'
'0010-RX-51-Symbols-Bind-braces-to-fourth-level-N-and-M.patch')

md5sums=('104ebf9bb46b6c55723e910832eff863'
         'c05ec8e2e235f677f124cc21eebb8870'
         'c57d5a0ed59d138f017fa52b8d27d53a'
         '0bd892c4c273ee3a81ccaad47ab09b51'
         '744208e54e09bc9f25d38a18c4f5f93c'
         'f45b93732a0edc52683f9f72d2279303'
         '8f739e1452a87ee2952d338eb7436693'
         'bc628629cbcd0691bb4a2cb8c7481a5d'
         '1ba24e1381d1802e3ae5db1bdd29c5ba'
         '986ddb0adbac94d0a394acd12760000f')

_gitroot="git://anongit.freedesktop.org/git/xkeyboard-config"
_gitname="xkeyboard-config"

build() {
  cd "${srcdir}/"
  msg "Connecting to GIT server...."

  if [ -d ${_gitname} ]; then
     cd ${_gitname} && git pull origin
     msg "The local files are updated."
  else
     git clone ${_gitroot} ${_gitname}
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting install..."

  cp -r "${srcdir}/${_gitname}" "${srcdir}/${_gitname}-build"

  cd "${srcdir}/${_gitname}-build/"

  git am ${srcdir}/*.patch

  ./autogen.sh
  ./configure --prefix=/usr \
              --with-xkb-base=/usr/share/X11/xkb \
              --with-xkb-rules-symlink=xorg \
              --enable-compat-rules=yes
  make
}

package() {
  cd "${srcdir}/$_gitname-build/"
  make DESTDIR="${pkgdir}" install
  rm -f "${pkgdir}/usr/share/X11/xkb/compiled"
  install -m755 -d "${pkgdir}/var/lib/xkb"
  install -m755 -d "${pkgdir}/usr/share/licenses/${pkgname}"
  install -m644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/"
}
