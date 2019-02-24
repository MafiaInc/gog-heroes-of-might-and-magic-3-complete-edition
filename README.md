# gog-heroes-of-might-and-magic-3-complete-edition
Heroes of Might &amp; Magic III and its expansions: Armageddon's Blade and The Shadow of Death

This is modified version of AUR package https://aur.archlinux.org/packages/gog-heroes-of-might-and-magic-3-complete-edition
* Updated from version 4.0.10665 to **4.0.25024**
* Uses unionfs-fuse to make data branch of original game's data to make it writable for the user
* Add [HoMM 3 HD patch](https://sites.google.com/site/heroes3hd/) by Alexander Barinov (aka baratorch, Bara)
  * Please use "original" branch for the version without the [HoMM 3 HD patch](https://sites.google.com/site/heroes3hd/)

## Installation instructions
1. Install [GOG Downloader](https://github.com/Sude-/lgogdownloader)
  * Run it once to setup login credentials to your GOG account
2. Clone this repo and build:
```
git clone https://github.com/MafiaInc/gog-heroes-of-might-and-magic-3-complete-edition.git
makepkg
```
3. Install
```
pacman -U gog-heroes-of-might-and-magic-3-complete-edition-4.0.25024-2-x86_64.pkg.tar.xz
```
