# Maintainer Erik Inkinen <erik.inkinen@gmail.com>
# Contributor: Chupligin Sergey (NeoChapay) <neochapay@gmail.com>
pkgname=sensorfw-hybris

pkgver=0.11.8.5.r0.g8f5ba90
pkgrel=1
pkgdesc="Sensor Framework provides an interface to hardware sensor drivers through logical sensors"
arch=('x86_64' 'aarch64')
url="https://github.com/droidian/sensorfw"
license=('LGPLv2')
depends=('libngf-git' 'libiphb-git' 'mlite' 'deviceinfo')
makedepends=('git' 'doxygen')
provides=("${pkgname%-hybris}")
conflicts=("${pkgname%-hybris}")
source=("${pkgname}::git+${url}")
md5sums=('SKIP')

prepare() {
    cd "${srcdir}/${pkgname}"
    unset LD_AS_NEEDED
    export LD_RUN_PATH=/usr/lib/sensord-qt5/
}

build() {
    cd "${srcdir}/${pkgname}"
    qmake \
    CONFIG+="mce" CONFIG+="configs" \
    CONFIG+="autohybris" CONFIG+="binder" \
    PC_VERSION=`echo $pkgver | sed 's/+.*//'` \
    QMAKE_CXXFLAGS+="-I/usr/lib/glib-2.0/include"
    make
}

package() {
    cd "${srcdir}/${pkgname}"
    make -j 1 INSTALL_ROOT="$pkgdir/" install
    mv $pkgdir/usr/sbin/* $pkgdir/usr/bin/
    rm -rf $pkgdir/usr/sbin/

    mkdir -p $pkgdir/usr/lib/systemd/system/
    mkdir -p $pkgdir/etc/systemd/system/sensorfwd.service.d/
    cp ./rpm/sensorfwd.service $pkgdir/usr/lib/systemd/system/
    cp ./rpm/sensorfwd-hybris-dropin.conf $pkgdir/etc/systemd/system/sensorfwd.service.d/
}
