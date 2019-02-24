pkgname=gog-heroes-of-might-and-magic-3-complete-edition
pkgver=4.0.25024
pkgrel=2
_gamename=${pkgname#gog-}
_gamename=${_gamename//-/_}
_gamehdpatchname="HoMM3%20HD%20Latest.exe"

pkgdesc="Heroes of Might & Magic III and its expansions: Armageddon's Blade and The Shadow of Death (with optional HD patch)"
arch=("i686" "x86_64")
url="https://www.gog.com/game/heroes_of_might_and_magic_3_complete_edition"
license=("custom")
groups=("games")

source=("setup_homm3_complete_${pkgver}.exe::gogdownloader://${_gamename}/en1installer0"
        "setup_homm3_complete_${pkgver}-1.bin::gogdownloader://${_gamename}/en1installer1"
        "http://ih875403.vds.myihor.ru/HoMM3_HD_Latest_setup.exe"
        "${pkgname}"
        "${pkgname}-campaign-editor"
        "${pkgname}-map-editor")
sha256sums=('76af0bfa9ddb1889ff0fe516599f21e3f2a696dd5baaf74333b5b58cc425a0e1'
            'a7b6d9f13bc6924d515eaa39430b290b80fd4253b2d1c40e9d936c4d242a466f'
            'SKIP'
            '3b4c50a772aaeabde4763d407387ef2213036a49a6cd7eead34bc5ddb1586a5d'
            '1c780da9550edf46168b44dca34b68a8159e3083f8960b5443721bfad3d87a06'
            '14927c0c59520861fd8a00d05f4b9fbcc70a71ff669add852ba0f5c670f8556a')
depends=(wine unionfs-fuse util-linux)

# Register lgogdownloader as a download agent
DLAGENTS+=('gogdownloader::/usr/bin/lgogdownloader --download-file=%u -o %o')

makedepends=("lgogdownloader>=2.25")

build() {
  export WINEDEBUG=-all
  export WINEPREFIX="$srcdir"/tmp/env
  export XDG_DATA_HOME="$srcdir"/tmp/local

  install -m755 -d "$srcdir"/tmp "$srcdir"/tmp/env "$srcdir"/tmp/local
  msg "Running GOG installer (Don't customize or launch game)"
  wine "${srcdir}/setup_homm3_complete_${pkgver}.exe"
  msg "Running HD patch installer (Don't customize or launch game)"
  wine "${srcdir}/HoMM3_HD_Latest_setup.exe" "/DIR=C:\GOG Games\HoMM 3 Complete"
  # Unfortunately, /verysilent doesn't work
  # messagebox from showing up and blocking. Manual intervention is required
#  msg "Installing HD patch"
#  wine "${srcdir}/${_gamehdpatchname}" /verysilent
}

package() {
  # Install license
  install -Dm644 "${srcdir}/tmp/env/drive_c/GOG Games/HoMM 3 Complete/EULA.txt" "${pkgdir}"/usr/share/licenses/$pkgname/LICENSE

  # Install game
  install -m755 -d "${pkgdir}"/opt/gog/
  # mv instead of cp to save disk space
  mv "${srcdir}/tmp/env/drive_c/GOG Games/HoMM 3 Complete" ${pkgdir}/opt/gog/${pkgname#gog-}
  # Remove unneeded files
  rm -rf ${pkgdir}/opt/gog/${pkgname#gog-}/unins*
  rm -rf ${pkgdir}/opt/gog/${pkgname#gog-}/*.lnk
  
  # Install .desktop file and icons
  install -Dm644 "${srcdir}/tmp/local/applications/wine/Programs/GOG.com/Heroes of Might and Magic 3 Complete/Heroes of Might and Magic 3 Complete.desktop" ${pkgdir}/usr/share/applications/${pkgname}.desktop
  install -Dm644 "${srcdir}/tmp/local/applications/wine/Programs/GOG.com/Heroes of Might and Magic 3 Complete/Tools/Map Editor.desktop"                  ${pkgdir}/usr/share/applications/${pkgname}-map-editor.desktop
  install -Dm644 "${srcdir}/tmp/local/applications/wine/Programs/GOG.com/Heroes of Might and Magic 3 Complete/Tools/Campaign Editor.desktop"             ${pkgdir}/usr/share/applications/${pkgname}-campaign-editor.desktop
  for res in 16 32 48 256
  do
    install -Dm644 ${srcdir}/tmp/local/icons/hicolor/${res}x${res}/apps/303B_goggame-1207658787.0.png ${pkgdir}/usr/share/icons/hicolor/${res}x${res}/apps/${pkgname}.png
  done
  for res in 16 32 48 64
  do
    install -Dm644 ${srcdir}/tmp/local/icons/hicolor/${res}x${res}/apps/FE0B_h3maped.0.png ${pkgdir}/usr/share/icons/hicolor/${res}x${res}/apps/${pkgname}-map-editor.png
    install -Dm644 ${srcdir}/tmp/local/icons/hicolor/${res}x${res}/apps/8095_h3ccmped.0.png ${pkgdir}/usr/share/icons/hicolor/${res}x${res}/apps/${pkgname}-campaign-editor.png
  done
  # Edit out wine jumbled entries from .desktop file
  for file in ${pkgname} ${pkgname}-map-editor ${pkgname}-campaign-editor
  do
    sed -i "/^Path=\|^Version=\|^Terminal=\|^Categories/d" ${pkgdir}/usr/share/applications/${file}.desktop
    sed -i "s,^\(Exec=\|Icon=\).*,\1${file}," ${pkgdir}/usr/share/applications/${file}.desktop
    printf "Version=${pkgver}\nTerminal=false\nCategories=Game;\n" >> ${pkgdir}/usr/share/applications/${file}.desktop
  done
  
  # Install startup scripts
  install -m755 -d ${pkgdir}/usr/bin
  for file in ${pkgname} ${pkgname}-map-editor ${pkgname}-campaign-editor
  do
    install -m755 ${srcdir}/${file} ${pkgdir}/usr/bin
  done
}

# vim:set ts=2 sw=2 et:
