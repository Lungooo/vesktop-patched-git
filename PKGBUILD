# Maintainer: Sophie Langer <sophie@lungoo.dev>
_pkgname=vesktop
pkgname=vesktop-patched-git
pkgdesc="A standalone Electron-based Discord app with Vencord & improved Linux support"
pkgver=1.5.8.r2.g27293d4
pkgrel=1

arch=("x86_64" "aarch64")
url="https://github.com/Vencord/Vesktop"
license=('GPL-3.0-only')

depends=('alsa-lib' 'gtk3' 'nss')
makedepends=('git' 'pnpm' 'npm' 'electron-vesktop')
optdepends=(
  'libnotify: Notifications'
  'xdg-utils: Open links, files, etc'
)

provides=("vesktop")
conflicts=('vesktop')

source=("$_pkgname::git+$url.git" "vesktop.desktop" "vesktop.sh" "afterPack.js")

sha256sums=('SKIP'
            '455c00b862aa0a7e18ca8e23d65d5c5ee4506cdfb15f1bf6f622cce39827de46'
            '506c246328af639d6f6a3e52215c7b34af2a6df11d195de6f57a8bbee750cce9'
            '122b17ce996318e533e6f2ab1c9b2961b39c3eba271c9b40f10c0da5dd738baa')

pkgver() {
  cd "$_pkgname"
  git describe --long --tags --abbrev=7 --exclude='*[a-zA-Z][a-zA-Z]*' \
    | sed -E 's/^[^0-9]*//;s/([^-]*-g)/r\1/;s/-/./g'
}


build() {
  cd "$srcdir/$_pkgname"

  # Add unpacked icon extraction script.
  sed -i '/"beforePack": "scripts\/build\/sandboxFix.js",/a\ \ \ \ \ \ \ \ "afterPack": "'$srcdir'/afterPack.js",' package.json

  # Use system's electron
  sed -i "/linux/s/^/        \"electronDist\": \"\\/usr\\/lib\\/electron-vesktop\",\n/" package.json

  # Fix publish because of ci detection
  sed -zi 's/"publish": *{\s*"provider": *"github"\s*}/"publish": \[\]/g' package.json

  pnpm i --frozen-lockfile
  pnpm package:dir
}

package() {
  cd "$srcdir/$_pkgname"

  # Create necessary directories
  install -d "$pkgdir/usr/lib/$_pkgname"
  install -d "$pkgdir/usr/bin"

  cp -R "dist/linux-unpacked/." "$pkgdir/usr/lib/$_pkgname"

  install -Dm644 "../vesktop.desktop" "$pkgdir/usr/share/applications/vesktop.desktop" # Install desktop entry
  install -Dm644 "LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE" # Install license
  for _icons in 1024 512 256 128 64 48 32 16; do
    install -Dm644 "dist/.icon-set/icon_${_icons}.png" "$pkgdir/usr/share/icons/hicolor/${_icons}x${_icons}/apps/$_pkgname.png"
  done # Install icons

  install -Dm755 "../vesktop.sh" "$pkgdir/usr/bin/$_pkgname" # Start script
}
