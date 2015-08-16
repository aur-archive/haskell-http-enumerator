# custom variables
_hkgname=http-enumerator
_licensefile=LICENSE

# PKGBUILD options/directives
pkgname=haskell-http-enumerator
pkgver=0.7.0
pkgrel=6
pkgdesc="HTTP client package with enumerator interface and HTTPS support."
url="http://github.com/snoyberg/http-enumerator"
license=("BSD3")
arch=('i686' 'x86_64')
makedepends=()
depends=("ghc=7.0.3-2"
         "haskell-asn1-data=0.5.1-23"
         "haskell-attoparsec=0.9.1.2-1"
         "haskell-attoparsec-enumerator=0.2.0.4-23"
         "haskell-base64-bytestring=0.1.0.3-1"
         "haskell-blaze-builder=0.3.0.1-18"
         "haskell-blaze-builder-enumerator=0.2.0.3-2"
         "haskell-case-insensitive=0.3.0.1-2"
         "haskell-certificate=0.9.1-5"
         "haskell-cprng-aes=0.2.1-24"
         "haskell-data-default=0.3.0-1"
         "haskell-enumerator=0.4.15-1"
         "haskell-failure=0.1.0.1-18"
         "haskell-http-types=0.6.6-1"
         "haskell-monad-control=0.2.0.3-2"
         "haskell-network=2.3.0.2-2.1"
         "haskell-tls=0.7.2-6"
         "haskell-tls-extra=0.3.1-6"
         "haskell-transformers=0.2.2.0-3.1"
         "haskell-utf8-string=0.3.6-7.2"
         "haskell-zlib-enum=0.2.1-23")
options=('strip')
source=("http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz")
install="${pkgname}.install"
sha256sums=("ff2a5673b48e34734a452560b5306d45bef52e02ad0cd892533579b4ff007941")

# PKGBUILD functions
build() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    
    runhaskell Setup configure -O -p --enable-split-objs --enable-shared \
        --prefix=/usr --docdir=/usr/share/doc/${pkgname} \
        --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock
    runhaskell Setup register --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    install -D -m744 register.sh   ${pkgdir}/usr/share/haskell/${pkgname}/register.sh
    install    -m744 unregister.sh ${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh
    install -d -m755 ${pkgdir}/usr/share/doc/ghc/html/libraries
    ln -s /usr/share/doc/${pkgname}/html ${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}
    runhaskell Setup copy --destdir=${pkgdir}
    install -D -m644 ${_licensefile} ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
    rm -f ${pkgdir}/usr/share/doc/${pkgname}/${_licensefile}
}
