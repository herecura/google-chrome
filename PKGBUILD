# Maintainer: Det <nimetonmaili at gmail a-dot com>
# Contributors: t3ddy, Lex Rivera aka x-demon, ruario

# Check for new Linux releases in: http://googlechromereleases.blogspot.com/search/label/Stable%20updates
# or use: $ curl -s https://dl.google.com/linux/chrome/rpm/stable/x86_64/repodata/other.xml.gz | gzip -df | awk -F\" '/pkgid/{ sub(".*-","",$4); print $4": "$10 }'

pkgname=google-chrome
pkgver=28.0.1500.71
pkgrel=1
pkgdesc="An attempt at creating a safer, faster, and more stable browser (Stable Channel)"
arch=('i686' 'x86_64')
url="https://www.google.com/chrome"
license=('custom:chrome')
depends=('alsa-lib' 'gconf' 'gtk2' 'hicolor-icon-theme' 'libpng' 'libxslt' 'libxss' 'nss' 'ttf-font' 'xdg-utils')
optdepends=('kdebase-kdialog: needed for file dialogs in KDE'
            'ttf-google-fonts-git')
provides=("google-chrome=$pkgver")
conflicts=('google-chrome')
options=('!emptydirs' '!strip')
install=$pkgname.install
_channel=stable
_arch=i386
[ "$CARCH" = 'x86_64' ] && _arch=amd64
source=("google-chrome-${_channel}_${pkgver}_${_arch}.deb::https://dl.google.com/linux/direct/google-chrome-${_channel}_current_${_arch}.deb"
        "$url/intl/en/eula_text.html")
md5sums=('19ec74343b5396760d5d6c88d7282cfb'
         '6d57da7476a4b1b7a81821d9c036425c')
[ "$CARCH" = 'x86_64' ] && md5sums[0]='03de683b0e9b8bf77db17060c8113d4d'


package() {
  msg2 "Extracting the data.tar.lzma"
  bsdtar -xf data.tar.lzma -C "$pkgdir/"

  msg2 "Moving stuff in place"
  mv "$pkgdir"/opt/google/chrome/google-chrome.desktop "$pkgdir"/usr/share/applications/

  # Icons
  for i in 16 22 24 32 48 64 128 256; do
    install -Dm644 "$pkgdir"/opt/google/chrome/product_logo_${i}.png "$pkgdir"/usr/share/icons/hicolor/${i}x${i}/apps/google-chrome.png
  done

  # Man page
  gzip "$pkgdir"/usr/share/man/man1/google-chrome.1

  # License
  install -Dm644 eula_text.html "$pkgdir"/usr/share/licenses/google-chrome/eula_text.html

  msg2 "Symlinking missing Udev lib"
  ln -s /usr/lib/libudev.so.1 "$pkgdir"/opt/google/chrome/libudev.so.0

  msg2 "Removing the Debian-intended cron job and duplicated images"
  rm "$pkgdir"/etc/cron.daily/google-chrome "$pkgdir"/opt/google/chrome/product_logo_*
  
  msg2 "Working around ~/libpeerconnection.log"
  sed '/exec/i \# Workaround for the creation of libpeerconnection.log in $HOME\ncd /tmp\n' -i "$pkgdir"/opt/google/chrome/google-chrome
}