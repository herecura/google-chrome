# Maintainer: Det <nimetonmaili at gmail a-dot com>
# Contributors: t3ddy, Lex Rivera aka x-demon, ruario

# Check for new Linux releases in: http://googlechromereleases.blogspot.com/search/label/Stable%20updates
# or use: $ curl -s https://dl.google.com/linux/chrome/rpm/stable/x86_64/repodata/other.xml.gz | gzip -df | awk -F\" '/pkgid/{ sub(".*-","",$4); print $4": "$10 }'

pkgname=google-chrome
pkgver=62.0.3202.89
pkgrel=1
pkgdesc="An attempt at creating a safer, faster, and more stable browser (Stable Channel)"
arch=('x86_64')
url="https://www.google.com/chrome"
license=('custom:chrome')
depends=('alsa-lib' 'gconf' 'gtk3' 'libcups' 'libxss' 'libxtst' 'nss')
optdepends=('kdialog: needed for file dialogs in KDE'
            'gnome-keyring: for storing passwords in GNOME keyring'
            'kwallet: for storing passwords in KWallet'
            'libunity: download progress on KDE'
            'ttf-liberation: fix fonts for some PDFs (CRBug #369991)'
            'xdg-utils')
provides=("google-chrome=$pkgver")
options=('!emptydirs' '!strip')
_channel=stable
_arch=amd64
source=(
	"google-chrome-${_channel}_${pkgver}_amd64.deb::http://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-${_channel}/google-chrome-${_channel}_${pkgver}-1_amd64.deb"
	'http://www.google.com/chrome/intl/en/eula_text.html'
)
sha256sums=('b9b9a34cc9bf625d8251b1142881524409f2132d29c59853afa7e033dacf8b16'
            'e93c01576427cad9099f2cf0df0be70d0a2cc0a3a66c743318b2138aa7c4ed93')
noextract=(
	"google-chrome-${_channel}_${pkgver}_amd64.deb"
)

prepare() {
	ar x "google-chrome-${_channel}_${pkgver}_${_arch}.deb"
}

package() {
	bsdtar -xf data.tar.xz -C "$pkgdir/"

	# Icons
	for i in 16 22 24 32 48 64 128 256; do
		install -Dm644 "$pkgdir"/opt/google/chrome/product_logo_$i.png \
			"$pkgdir"/usr/share/icons/hicolor/${i}x$i/apps/google-chrome.png
	done

	# Man page
	gzip "$pkgdir"/usr/share/man/man1/google-chrome.1

	# License
	install -Dm644 eula_text.html "$pkgdir"/usr/share/licenses/google-chrome/eula_text.html

	# fix chrome to use libudev.so.1
	sed -e 's/libudev.so.0/libudev.so.1/g' \
		-i "$pkgdir/opt/google/chrome/chrome"

	_name=$(echo ${source/_*} | sed 's/.*/\u&/')
	sed -i "/Exec=/i\StartupWMClass=$_name" "$pkgdir"/usr/share/applications/google-chrome.desktop

	sed -i 's/ "$@"/"$CHROMIUM_USER_FLAGS" "$@"/' "$pkgdir"/opt/google/chrome/google-chrome

	rm -r "$pkgdir"/etc/cron.daily/ "$pkgdir"/opt/google/chrome/cron/
	rm "$pkgdir"/opt/google/chrome/product_logo_*.png
}

