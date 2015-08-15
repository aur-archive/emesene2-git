# Contributor: Claudio Riva <firetux83@gmail.com>

# http://github.com/marianoguerra/emesene/tree/master

pkgname=emesene2-git
_realname=emesene2
pkgver=20121120
pkgrel=1
pkgdesc="An OS independent MSN Messenger client writed in python and GTK"
arch=('i686' 'x86_64')
url="http://www.emesene.org/"
license=('PSF' 'GPL' 'LGPL2')
depends=('python2' 'pygtk' 'python2-dnspython' 'python2-pylint' 'python2-notify' 
         'openssl' 'papyon' 'python2-imaging' 'python2-dbus' 'xorg-xprop' 'libnotify')
optdepends=('pywebkitgtk: for alternative conversation window'
            'gobject-introspection: for GTK3 version')
makedepends=('git')
source=('emesene2.run' 'emesene2.desktop')
md5sums=('02b5dd16f271b051dd2b3c5538055a9b'
         'a4bfcf1b89018a3aad09f11367589e3b')

_gitroot="git://github.com/emesene/emesene.git"
_gitname="emesene"

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [ -d $_gitname ] ; then
    cd $_gitname && git pull origin master
    git submodule update
    msg "The local files are updated."
  else
    git clone $_gitroot
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting make..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"
  git submodule init
  git submodule update

  cd emesene
  
  sed -i '/import e3dummy/d' emesene.py
}

package() {
  cd "$srcdir/$_gitname-build/emesene"
  

  # Copying files
  mkdir -p $pkgdir/usr/{bin,share/${_realname}}
  cp -r * $pkgdir/usr/share/${_realname}
  rm -rf $pkgdir/usr/share/${_realname}/docs
  
  # install startup file
  install -Dm755 $srcdir/emesene2.run $pkgdir/usr/bin/${_realname}
  
  # install pixmap
  install -Dm644 themes/images/default/logo.png $pkgdir/usr/share/pixmaps/${_realname}.png
  
  # install desktop file
  install -Dm644 $srcdir/${_realname}.desktop $pkgdir/usr/share/applications/${_realname}.desktop
  
  # Removing .git files
  rm -rf `find $pkgdir/ -type d -name .git`
  
  # Removing build folder
  rm -rf "$srcdir/$_gitname-build"
} 
