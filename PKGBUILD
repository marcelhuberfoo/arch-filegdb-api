# Maintainer: SaultDon < sault[] don atgmail []com >
pkgname=filegdb-api
_pkgname=FileGDB_API
pkgver=1.3
pkgrel=1
pkgdesc="A proprietary API for reading/writing an ESRI FileGDB"
arch=('i686' 'x86_64')
url="http://www.esri.com/apps/products/download/#File_Geodatabase_API_1.3"
license=('custom:"ESRI - User Restrictions"')
makedepends=('libxml2' 'libxml++') #FIXME not sure if C bindings needed for ProcessTopology, libxml2 header is found in both these packages...
optdepends=('gdal-filegdb: wrapper')
changelog=$pkgname.changelog
install=$pkgname.install
case $CARCH in
i686)
  source=($pkgname-$pkgver.tar.gz::http://downloads2.esri.com/Software/${_pkgname}_${pkgver//./_}-32.tar.gz)
  md5sums=('f54739d309436f96d0a23149bf08ac53')
  ;; 
x86_64)
  source=($pkgname-$pkgver.tar.gz::http://downloads2.esri.com/Software/${_pkgname}_${pkgver//./_}-64.tar.gz)
  md5sums=('167ed3d756ad961c0849a9387f4be733')
  ;; 
esac
noextract=($pkgname-$pkgver.tar.gz)

build() {
	tar xzvf $pkgname-$pkgver.tar.gz
	#Setup LD_LIBRARY_PATH
	LD_LIBRARY_PATH=$srcdir/${_pkgname}/lib:$LD_LIBRARY_PATH
        export LD_LIBRARY_PATH
        #Building all samples
	cd "$srcdir/${_pkgname}/samples"
	make -j9
	# Building ProcessTopology
	cd "$srcdir/${_pkgname}/samples/ProcessTopologies"
	# Insert libxml2 library path to Makefile
	sed -i '/^CXXFLAGS=/ s/$/ -I\/usr\/include\/libxml2\//' Makefile
	make
}

# Uncomment check() portion if you want to perform sample tests
# Warning: Lots of verbose output for tests!
#
#check() {
#	cd "$srcdir/${_pkgname}/samples/bin"
#	for i in *
#	do
#	  ./${i}
#	done
#}

package() {
	mkdir -p $pkgdir/usr/{lib,include,share/{doc,licenses}}/$pkgname
	mkdir -p $pkgdir/usr/lib/$pkgname/{lib,include}
	mkdir -p $pkgdir/etc/ld.so.conf.d
	install -Dm644 $srcdir/${_pkgname}/license/* "$pkgdir/usr/share/licenses/$pkgname/"
	install -Dm644 $srcdir/${_pkgname}/lib/* "$pkgdir/usr/lib/$pkgname/lib/"
	install -Dm644 $srcdir/${_pkgname}/include/* "$pkgdir/usr/lib/$pkgname/include/"
	cp -r $srcdir/${_pkgname}/doc/html "$pkgdir/usr/share/doc/$pkgname/"
	echo "/usr/lib/$pkgname/lib" > $pkgdir/etc/ld.so.conf.d/$pkgname.conf
	find $pkgdir/usr/share/doc/$pkgname/ -type d -exec chmod 755 '{}' \;
	find $pkgdir/usr/share/doc/$pkgname/ -type f -exec chmod 644 '{}' \;
	chown root: $pkgdir/usr/share/doc/$pkgname/*
	chmod 644 $pkgdir/etc/ld.so.conf.d/$pkgname.conf
	chown root: $pkgdir/etc/ld.so.conf.d/$pkgname.conf
}
