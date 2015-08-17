# Contributor: Heeru Kiyura <puce@gmx.com>
pkgname=ratpoison-xft
pkgver=20080525
pkgrel=1
pkgdesc="Ratpoison from git, built with Xft"
arch=('i686' 'x86_64')
url="http://www.nongnu.org/ratpoison/"
license=('GPL')
depends=('libxinerama' 'readline' 'bash' 'perl' 'libxtst' 'libxft')
makedepends=('git')
provides=('ratpoison')
replaces=('ratpoison')
source=()
md5sums=()

_gitroot="git://git.savannah.nongnu.org/ratpoison.git"
_gitname="ratpoison"

build() {
  cd ${startdir}/src

 ## Git checkout
  if [ -d ${startdir}/src/${_gitname} ] ; then
    msg "Git checkout:  Updating existing tree"
    cd ${_gitname} && git-pull origin  || return 1
    msg "Git checkout:  Tree has been updated"
  else
    msg "Git checkout:  Retrieving sources"
    git clone ${_gitroot}  || return 1
  fi
  msg "Checkout completed"

 ## Build
  rm -rf ${startdir}/src/${_gitname}-build
  cp -r ${startdir}/src/${_gitname} ${startdir}/src/${_gitname}-build  || return 1
  cd ${startdir}/src/${_gitname}-build

  ./autogen.sh  || return 1
  ./configure --prefix=/usr --with-xft  || return 1
  make  || return 1

  cd contrib
  ./genrpbindings  || return 1
  cd ..

 ## Install
  install -d ${startdir}/pkg/usr/share/ratpoison/bindings

  make DESTDIR=${startdir}/pkg install  || return 1
  install -m 644 contrib/{Ratpoison.pm,ratpoison-cmd.el,ratpoison.rb,ratpoison.lisp,ratpoison.py} \
            ${startdir}/pkg/usr/share/ratpoison/bindings  || return 1

  chmod a+x $startdir/pkg/usr/share/ratpoison/{allwindows.sh,clickframe.pl,rpshowall.sh,rpws,split.sh}
} 
