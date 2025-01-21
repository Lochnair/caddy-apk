# Contributor: Michał Polański <michal@polanski.me>
# Maintainer: Michał Polański <michal@polanski.me>
pkgname=caddy
pkgver=2.9.1
pkgrel=1
pkgdesc="Fast, multi-platform web server with automatic HTTPS"
url="https://caddyserver.com/"
license="Apache-2.0"
arch="all"
depends="ca-certificates"
makedepends="xcaddy"
subpackages="$pkgname-openrc"
pkgusers="$pkgname"
pkggroups="$pkgname"
install="$pkgname.pre-install"
source="$pkgname.initd
	Caddyfile
	"
export GOCACHE="${GOCACHE:-"$srcdir/go-cache"}"
export GOTMPDIR="${GOTMPDIR:-"$srcdir"}"
export GOMODCACHE="${GOMODCACHE:-"$srcdir/go"}"

builddir="$srcdir"

build() {
	xcaddy build "v$pkgver" \
		--with https://github.com/caddy-dns/cloudflare
}

check() {
	if [ "$(./caddy --version)" = "unknown" ]; then
		error "caddy built without version info"
		return 1
	fi
}

package() {
	install -Dm755 caddy -t "$pkgdir"/usr/sbin/

	install -Dm755 "$srcdir"/$pkgname.initd "$pkgdir"/etc/init.d/$pkgname
	install -Dm644 "$srcdir"/Caddyfile -t "$pkgdir"/etc/$pkgname/
}

sha512sums="
d67725ba47c2e983825b9390eb3244d2d429bb649cf3898e279849226922f50ee4fc6bb5aeea20144a22da4ec6205d01235573713bf8c6c8243df09386fa2754  caddy.initd
d3110dd79f7d5e602a34d42569104dc97603994e42daf5f6b105303a3d034b52b91ef5fb156d5bf7b7a3a58ec0aeff58afc402618d0555af053771952a866f76  Caddyfile
"
