# Maintainer: Stefan Husmann <stefan-husmann@t-online.de>
# Contributor: Brad Fanella <bradfanella@archlinux.us>
# Contributor: Zerial <fernando@zerial.org>
# Contributor: Dalius <dagis@takas.lt>
# Contributor: Sergej Pupykin <pupykin.s+arch@gmail.com>
pkgname=amaya
pkgver=11.3.1
pkgrel=10
pkgdesc="W3C's WYSIWYG HTML Editor"
arch=('i686' 'x86_64')
url="http://www.w3.org/Amaya/"
license=('W3C')
depends=('wxgtk' 'libgl' 'raptor1' 'libxt')
makedepends=('chrpath')
options=('!makeflags')
install=$pkgname.install
source=(amaya-fix-amaya-wxfile.patch \
  amaya-fix-thotlib-png14.patch \
  amaya-fix-thotlib-wxfile.patch \
  amaya-splitmode.patch \
  amaya-wakeupidle.patch \
  amaya-wxyield.patch \
  explicite_linking.patch \
  http://www.w3.org/Amaya/Distribution/$pkgname-sources-$pkgver.tgz)
md5sums=('4e79692553e88de93a3f56c40dd442dc'
         '0418f3a614e6d0a8e27ae038c78d8c4d'
         '6501c87f7ab45e6c1a3ef214a6ed583e'
         'bc42d4b3ff7b43c8d0f7955dd1498b01'
         '32347b32aded742b705a2038416f74de'
         'c42175f9cc9e90277547828b9cf6a92a'
         '55687c985a1cab4a5d83f5aae9d06e34'
         '4a92b4e043fbd1add5b1e17fb7ed8755')

build() {
	cd $srcdir/Amaya$pkgver

	patch -p1 < $srcdir/amaya-fix-amaya-wxfile.patch
	patch -p1 < $srcdir/amaya-fix-thotlib-png14.patch
	patch -p1 < $srcdir/amaya-fix-thotlib-wxfile.patch
	patch -p1 < $srcdir/amaya-splitmode.patch
	patch -p1 < $srcdir/amaya-wakeupidle.patch
	patch -p1 < $srcdir/amaya-wxyield.patch
	patch -p1 < $srcdir/explicite_linking.patch

	cd Amaya
	if [ -d ./WX ]; then
	  rm -rf WX
	  mkdir WX
	fi
	cd WX

	if [ "$CARCH" = "x86_64" ] ; then
		[ $NOEXTRACT -eq 1 ] || cp ../../Mesa/configs/linux-x86-64 \
		  ../../Mesa/configs/current
	else
		[ $NOEXTRACT -eq 1 ] || cp ../../Mesa/configs/linux-x86 \
		  ../../Mesa/configs/current
	fi
	../configure --prefix=/usr/share --exec=/usr/share \
	  --datadir=/usr/share --enable-system-raptor --enable-system-wx

	 make
}

package() {
	cd $srcdir/Amaya$pkgver/Amaya/WX

	install -d $pkgdir/usr/bin
	make prefix=$pkgdir/usr/share install

	ln -f -s ../share/Amaya/wx/bin/amaya $pkgdir/usr/bin/amaya
#	install -Dm644 $srcdir/Amaya$pkgver/Amaya/amaya/COPYRIGHT \
#		$pkgdir/usr/share/licenses/$pkgname/COPYRIGHT

	mkdir -p $pkgdir/usr/share/Amaya/lib
	cp -a $srcdir/Amaya$pkgver/Amaya/WX/Mesa/lib/libGL.so.1* $pkgdir/usr/share/Amaya/lib/
	cp -a $srcdir/Amaya$pkgver/Amaya/WX/Mesa/lib/libGLU.so.1* $pkgdir/usr/share/Amaya/lib/
	chrpath -r /usr/share/Amaya/lib $pkgdir/usr/share/Amaya/wx/bin/amaya_bin
	chrpath -r /usr/share/Amaya/lib $pkgdir/usr/share/Amaya/wx/bin/print
}
