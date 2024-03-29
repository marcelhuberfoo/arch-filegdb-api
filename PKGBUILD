# Maintainer: SaultDon < sault[] don atgmail []com >
pkgname=filegdb-api
_pkgname=FileGDB_API
pkgver=1.3
pkgrel=2
pkgdesc="A proprietary API for reading/writing an ESRI FileGDB"
arch=('i686' 'x86_64')
url="http://www.esri.com/apps/products/download/#File_Geodatabase_API_1.3"
license=('custom:"ESRI - User Restrictions"')
makedepends=('libxml2' 'libxml++') #FIXME not sure if C bindings needed for ProcessTopology, libxml2 header is found in both these packages...
optdepends=('gdal-filegdb: wrapper')
changelog=$pkgname.changelog
install=$pkgname.install
sha256sums=('1ce57249035eaa4aba9ac1ac9494adffe92ebac8bd3bedff345ac7760e604f0d')
case $CARCH in
i686)
  source=($pkgname.tar.gz::http://downloads2.esri.com/Software/${_pkgname}_${pkgver//./_}-32.tar.gz)
  ;; 
x86_64)
  source=($pkgname.tar.gz::http://downloads2.esri.com/Software/${_pkgname}_${pkgver//./_}-64.tar.gz)
  ;; 
esac

build() {
  #Setup LD_LIBRARY_PATH
  LD_LIBRARY_PATH=$srcdir/${_pkgname}/lib:$LD_LIBRARY_PATH
  export LD_LIBRARY_PATH
  export CPPFLAGS=-Dlinux
  #Building all samples
  cd "$srcdir/$_pkgname/samples"
  make -j9
  # Building ProcessTopology
  cd "$srcdir/$_pkgname/samples/ProcessTopologies"
  # Insert libxml2 library path to Makefile
  sed -i '/^CXXFLAGS=/ s/$/ -I\/usr\/include\/libxml2\//' Makefile
  make
}

# Warning: Lots of verbose output for tests!
check() {
  cd "$srcdir/$_pkgname/samples/bin"
  for i in *
  do
    ./${i}
  done
}

package() {
  cd "$srcdir/$_pkgname"
  install -d -m755 \
    "$pkgdir"/usr/lib/$pkgname/{lib,include} \
    "$pkgdir"/usr/share/{doc,licenses}/$pkgname \
    "$pkgdir"/etc/ld.so.conf.d
  install -m444 $srcdir/$_pkgname/license/* "$pkgdir/usr/share/licenses/$pkgname/"
  install -m555 $srcdir/$_pkgname/lib/* "$pkgdir/usr/lib/$pkgname/lib/"
  install -m444 $srcdir/$_pkgname/include/* "$pkgdir/usr/lib/$pkgname/include/"
  cp -r $srcdir/$_pkgname/doc/html "$pkgdir/usr/share/doc/$pkgname/"
  echo "/usr/lib/$pkgname/lib" > $pkgdir/etc/ld.so.conf.d/$pkgname.conf
  chmod 644 $pkgdir/etc/ld.so.conf.d/$pkgname.conf
}

# vim:set ts=2 sw=2 et:
