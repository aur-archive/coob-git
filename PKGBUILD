pkgname=coob-git
pkgver=124
pkgrel=1
pkgdesc="A CubeWorld Server"
arch=(i686 x86_64)
license=("GPLv2")
depends=(mono)
makedepends=(git)
url="https://github.com/amPerl/Coob"
source=("${pkgname%-*}::git+https://github.com/amPerl/Coob.git"
"jint_path_fix.diff"
"remove_unbuildables.diff"
"coob.sh")
conflicts=(coob)
provides=(coob)
md5sums=('SKIP'
         'e580215b6743fa55100de0fd3feac695'
         '4249c472fd097f97b55e694117b5cd92'
         '460e1c98d7558e52619cb19d9cd60edf')

pkgver() {
  cd "$srcdir/${pkgname%-*}"
  echo $(git rev-list --count master)
}

prepare() {
  cd "$srcdir/${pkgname%-*}"
  patch -Np0 Coob.sln < "$srcdir/remove_unbuildables.diff"
  patch -Np0 "Coob/Coob.csproj" < "$srcdir/jint_path_fix.diff"
}

build() {
  cd "$srcdir/${pkgname%-*}"
  xbuild "Coob.sln"
}

package() {
  cd "${srcdir}/${pkgname%-*}/Coob/bin/Debug"
  find . -name '*.exe' -o -name '*.config' -o -name '*.cs' -o -name '*.mdb' -o -name '*.dll' |
    xargs -rtl1 -I {} install -Dm644 {} "$pkgdir/srv/coob/"{}
  find "$pkgdir/srv/coob" -name '*.exe' -o -name '*.dll' | xargs -rtl1 mono --aot
  cd "../../../Content"
  cp -r Plugins "$pkgdir/srv/coob/"
  install -Dm644 "../LICENSE" "$pkgdir/usr/share/licenses/coob/LICENSE"
  install -Dm755 "$srcdir/coob.sh" "$pkgdir/usr/bin/coob"
}
