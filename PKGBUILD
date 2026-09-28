# Maintainer: xsigil <xsigil@users.noreply.github.com>
pkgname=epigraph-wall
pkgver=1.0.1
pkgrel=1
pkgdesc="Epigraph Wall - Wall layout and document generation system"
arch=('x86_64')
url="https://github.com/xsigil/epigraph-wall"
license=('MIT')
depends=('glibc')
makedepends=('go')
source=("${pkgname}-${pkgver}.tar.gz::https://github.com/xsigil/${pkgname}/archive/refs/tags/v${pkgver}.tar.gz")
sha256sums=('008791c847744f6597150bc0da9a8cb5c8a54ca2bab33b6b792d8bfdb4c1b319')

prepare() {
    cd "${pkgname}-${pkgver}"
    mkdir -p build
}

build() {
    cd "${pkgname}-${pkgver}"
    export CGO_CPPFLAGS="${CPPFLAGS}"
    export CGO_CFLAGS="${CFLAGS}"
    export CGO_CXXFLAGS="${CXXFLAGS}"
    export CGO_LDFLAGS="${LDFLAGS}"
    export GOFLAGS="-buildmode=pie -trimpath -ldflags=-linkmode=external -mod=readonly -modcacherw"

    # main.go がルートにある場合はカレントをビルド、cmd/ にエントリポイントがある場合はそちらを指定
    go build -o build/${pkgname} .
}

package() {
    cd "${pkgname}-${pkgver}"
    install -Dm755 "build/${pkgname}" "${pkgdir}/usr/bin/${pkgname}"

    if [ -f LICENSE ]; then
        install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
    fi

    # doc ディレクトリに man ページ等があれば配置
    if [ -d doc ]; then
        find doc -type f -name "*.[1-8]" -exec install -Dm644 {} "${pkgdir}/usr/share/man/man1/" \; 2>/dev/null || true
    fi
}
