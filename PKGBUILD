pkgname=monogame
pkgver=3.4
pkgrel=2
pkgdesc="MonoGame is an open source implementation of the Microsoft XNA 4.x Framework."
arch=(any)
license=("MSPL")
depends=(opentk sdl_gfx sdl_mixer sdl_ttf sdl_image sdl_net)
makedepends=(dos2unix mono)
optdepends=(gtk-sharp-3 sharpfont assimp-net ffmpeg nvidia-texture-tools)
url="http://www.monogame.net/"
conflicts=(tao-framework)
source=("https://github.com/mono/MonoGame/archive/v$pkgver.tar.gz"
"Dependencies.zip::https://github.com/Mono-Game/MonoGame.Dependencies/archive/6abfe822dceb3469494ac49075dd8a322e8e104c.zip"
"monogame.pc" "mgcb.sh" "pipeline.sh" "pipeline.desktop" "pipeline.png" "monogame-net.pc")
md5sums=('71cc08c46267a074e51031ea8600fb4b'
         'f4f85c05096cdba8f4891239235db317'
         '0f3f9882d92fc458c7d0e9731db02f3c'
         'a0ff773f5647651148ecb443a29b9ea9'
         '46a2caa68e21fd99aa4b30b78ab1a109'
         '5e67a0b9a7e0a6f98a34f92ad688cd6e'
         '2e1ad2d1171f049df3549d0b74b98c40'
         'dff90f7fc15310225a10f73f3d3420a7')

prepare() {
	cd MonoGame.Dependencies-*
	find . -type f -exec dos2unix {} \;
	cd ../MonoGame-$pkgver/ThirdParty/Dependencies
	cp -r ../../../MonoGame.Dependencies-*/* .
	cd ../..
	mono Protobuild.exe
}

build() {
  cd MonoGame-$pkgver
  xbuild MonoGame.Framework.Linux.sln
}

package() {
  cd MonoGame-$pkgver/MonoGame.Framework/bin/Linux/AnyCPU/Debug
  find . -name 'MonoGame.Framework.dll*' -exec install -Dm644 {} "$pkgdir/usr/lib/monogame/"{} \;
  find . -name 'MonoGame.Framework.Net.dll*' -exec install -Dm644 {} "$pkgdir/usr/lib/monogame/"{} \;
  find . -name 'Lidgren.Network.dll*' -exec install -m644 {} "$pkgdir/usr/lib/monogame/"{} \;
  find . -name 'Tao.Sdl.dll*' -exec install -m644 {} "$pkgdir/usr/lib/monogame/"{} \;
  cd "$srcdir/MonoGame-$pkgver/Tools/Pipeline/bin/Linux/AnyCPU/Debug"
  find . -name 'MonoGame.Framework.Content.Pipeline.dll*' -exec install -m644 {} "$pkgdir/usr/lib/monogame/"{} \;
  find . -name 'Nvidia.TextureTools.dll*' -exec install -m644 {} "$pkgdir/usr/lib/monogame/"{} \;
  find . -name 'MGCB.exe*' -exec install -m644 {} "$pkgdir/usr/lib/monogame/"{} \;
  find . -name 'Pipeline.exe*' -exec install -m644 {} "$pkgdir/usr/lib/monogame/"{} \;
  install -m644 ManagedPVRTC.dll "$pkgdir/usr/lib/monogame/"
  cd Templates
  find . -type f -exec install -Dm644 {} "$pkgdir/usr/lib/monogame/Templates/"{} \;
  cd "$pkgdir/usr/lib/monogame"
  gacutil -i Tao.Sdl.dll -root "$pkgdir/usr/lib"
  install -Dm644 "$srcdir/MonoGame-$pkgver/LICENSE.txt" "$pkgdir/usr/share/licenses/monogame/LICENSE.txt"
  install -Dm644 "$srcdir/monogame.pc" "$pkgdir/usr/lib/pkgconfig/monogame.pc"
  install -m644 "$srcdir/monogame-net.pc" "$pkgdir/usr/lib/pkgconfig/"
  install -Dm644 "$srcdir/pipeline.desktop" "$pkgdir/usr/share/applications/pipeline.desktop"
  install -Dm755 "$srcdir/mgcb.sh" "$pkgdir/usr/bin/mgcb"
  install -m755 "$srcdir/pipeline.sh" "$pkgdir/usr/bin/pipeline"
  install -Dm644 "$srcdir/pipeline.png" "$pkgdir/usr/share/icons/pipeline.png"
  sed -i "s|@VERSION@|$pkgver|" "$pkgdir/usr/lib/pkgconfig/monogame.pc"
  sed -i "s|@VERSION@|$pkgver|" "$pkgdir/usr/lib/pkgconfig/monogame-net.pc"
}
