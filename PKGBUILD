# Maintainer: Det <nimetonmaili at gmail a-dot com>
# Contributors: t3ddy, Lex Rivera aka x-demon, ruario

# Check for new Linux releases in: http://googlechromereleases.blogspot.com/search/label/Stable%20updates
# or use: $ curl -s https://dl.google.com/linux/chrome/rpm/stable/x86_64/repodata/other.xml.gz | gzip -df | awk -F\" '/pkgid/{ sub(".*-","",$4); print $4": "$10 }'

pkgname=google-chrome
pkgver=72.0.3626.109
pkgrel=1
pkgdesc="An attempt at creating a safer, faster, and more stable browser (Stable Channel)"
arch=('x86_64')
url="https://www.google.com/chrome"
license=('custom:chrome')
depends=('alsa-lib' 'gconf' 'gtk3' 'libcups' 'libxss' 'libxtst' 'nss')
optdepends=('kdialog: for file dialogs in KDE'
            'gnome-keyring: for storing passwords in GNOME keyring'
            'kwallet: for storing passwords in KWallet'
            'gtk3-print-backends: for printing'
            'libunity: for download progress on KDE'
            'ttf-liberation: fix fonts for some PDFs (CRBug #369991)'
            'xdg-utils')
provides=("google-chrome=$pkgver")
options=('!emptydirs' '!strip')
_channel=stable
source=(
	"google-chrome-${_channel}_${pkgver}_amd64.deb::http://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-${_channel}/google-chrome-${_channel}_${pkgver}-1_amd64.deb"
	'http://www.google.com/chrome/intl/en/eula_text.html'
)
sha256sums=('c8bf514f3c70868120924ee2d4872a0e47dcff315bdf799ba2f98dd7aa85fe2b'
            'e93c01576427cad9099f2cf0df0be70d0a2cc0a3a66c743318b2138aa7c4ed93')

package() {
    bsdtar -xf data.tar.xz -C "$pkgdir/"

    # Icons
    for i in 16x16 22x22 24x24 32x32 48x48 64x64 128x128 256x256; do
        install -Dm644 "$pkgdir"/opt/google/chrome/product_logo_${i/x*}.png \
            "$pkgdir"/usr/share/icons/hicolor/$i/apps/google-chrome.png
    done

    # Man page
    if [[ -f "$pkgdir"/usr/share/man/man1/google-chrome.1 ]]; then
        gzip "$pkgdir"/usr/share/man/man1/google-chrome.1
    fi

    # License
    install -Dm644 "$srcdir/eula_text.html" \
        "$pkgdir"/usr/share/licenses/google-chrome/eula_text.html

    sed -i "/Exec=/i\StartupWMClass=Google-chrome" \
        "$pkgdir"/usr/share/applications/google-chrome.desktop

    rm -r "$pkgdir"/etc/cron.daily/ "$pkgdir"/opt/google/chrome/cron/
    rm "$pkgdir"/opt/google/chrome/product_logo_*.png
}

