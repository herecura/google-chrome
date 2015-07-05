# Maintainer: t3ddy  <t3ddy1988 "at" gmail {dot} com>
# Contributor: Lex Rivera aka x-demon <aur@x-demon.org>
# Contributor: Det <nimetonmaili at gmail a-dot com>
# Contributor: ruario

pkgname=google-chrome
pkgver=24.0.1312.69   # Check for new Linux releases in: http://googlechromereleases.blogspot.com/search/label/Stable%20updates
pkgrel=1
pkgdesc="An attempt at creating a safer, faster, and more stable browser (Stable Channel)"
arch=('i686' 'x86_64')
url="http://www.google.com/chrome"
license=('custom:chrome')
depends=('alsa-lib' 'gconf' 'gtk2' 'hicolor-icon-theme' 'libpng' 'libxslt' 'libxss' 'nss' 'ttf-dejavu' 'xdg-utils')
optdepends=('kdebase-kdialog: needed for file dialogs in KDE')
provides=("google-chrome=$pkgver")
conflicts=('google-chrome')
install=${pkgname}.install
_channel='stable'
_verbld=24.0.1312.69-180721

if [ "$CARCH" = "i686" ]; then
    _arch='i386'
    md5sums=('7919aa97b93367bab496a3f4c9f57895')
elif [ "$CARCH" = "x86_64" ]; then
    _arch='x86_64'
    md5sums=('40bf207cb74e336a047eca843a66e19e')
fi


source=("http://dl.google.com/linux/chrome/rpm/stable/${_arch}/google-chrome-${_channel}-${_verbld}.${_arch}.rpm")


package() {
    msg "Preparing install"
    install -d "$pkgdir"/{opt,usr/{bin,share/applications}}
    mv opt/google "$pkgdir"/opt
    msg2 "Done preparing!"

    msg "Actual installation"
    ln -s /opt/google/chrome/google-chrome "$pkgdir/usr/bin/"
    mv "$pkgdir/opt/google/chrome/google-chrome.desktop" "$pkgdir/usr/share/applications"
    
    # Remove 64-bit executable from 32-bit installation
    if [ "$CARCH" = "i686" ]; then
        rm -f "$pkgdir/opt/google/chrome/nacl_irt_x86_64.nexe"
        rm -f "$pkgdir/opt/google/chrome/nacl_irt_srpc_x86_64.nexe"
    fi
    
    # Udev workaround
    ln -s /usr/lib/libudev.so.1 "$pkgdir/opt/google/chrome/libudev.so.0"

    # Adding man page
    if [ ! -e "$srcdir/usr/share/man/man1/google-chrome.1.gz" ]; then
      gzip -9 "$srcdir/usr/share/man/man1/google-chrome.1"
    fi
    install -Dm644 "$srcdir/usr/share/man/man1/google-chrome.1.gz" "$pkgdir/usr/share/man/man1/google-chrome.1.gz"

    # Symlinking icons to /usr/share/icons/hicolor/
    for i in 16x16 22x22 24x24 32x32 48x48 64x64 128x128 256x256; do
      mkdir -p "$pkgdir/usr/share/icons/hicolor/$i/apps/"
      ln -s /opt/google/chrome/product_logo_${i/x*}.png "$pkgdir/usr/share/icons/hicolor/$i/apps/google-chrome.png"
    done

    # Fixing permissions of chrome-sandbox
    chmod 4755 "$pkgdir/opt/google/chrome/chrome-sandbox"

    msg2 "Installation finished!"
}
