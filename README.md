## Hearthstone and HDT installation for Arch Linux
These instructions assume a clean [wine prefix](https://wiki.winehq.org/FAQ#Wineprefixes), and will use `~/.wine.hearthstone` as the prefix in example commands. If you already have such a prefix please either clean it or use a different one.

## Prerequisites
- wine-staging
- winetricks
- lib32-libldap and lib32-gnutls (without these the login button won't show up on Battle.net)

## Installation

### 0. Grab HDT oauth from windows
If you aren't going to login to hsreplay in HDT you can skip this.

HDT works on linux, however you can't login to hsreplay. However you can grab the login info from a working windows install and then use that on linux. 

First ensure that HDT is not running on windows. Next download the [`hdte.exe`](https://github.com/git-uname/hearthstone_hdt_archlinux/raw/master/hdte.exe) file from this repository on Windows. Then open the command prompt and run the following:

```shell
hdte.exe decrypt %AppData%\HearthstoneDeckTracker\hsreplay_oauth Downloads\hsreplay_oauth.decrypted
```

You will have to transfer that `hsreplay_oauth.decrypted` from `Downloads` to Linux (I'll assume it's in `~/Downloads` on linux later on when installing HDT) 

### 1. Setup the prefix
```shell
WINEPREFIX=~/.wine.hearthstone WINEARCH=win32 wine wineboot # Create 32 bit wine prefix
WINEPREFIX=~/.wine.hearthstone winetricks dotnet45 # Install .NET 4.5
WINEPREFIX=~/.wine.hearthstone winetricks win7 # Set Windows version to 7 (.NET install will have set it to 2k3)
WINEPREFIX=~/.wine.hearthstone winetricks corefonts # Install fonts for Battle.net
WINEPREFIX=~/.wine.hearthstone winetricks nocrashdialog # Without this popup warnings appear after running Battle.net
```

### 2. Install HDT

Download HDT from https://hsdecktracker.net/download/ then run 

```shell
WINEPREFIX=~/.wine.hearthstone wine ~/Downloads/HDT-Installer.exe # or wherever you downloaded it to
```

If you want to be logged in to hsreplay then download the [`hdte.exe`](https://github.com/borisbabic/hearthstone_hdt_linux/raw/master/hdte.exe) file from this repository to `~/Downloads` alongside the `hsreplay_oauth.decrypted` file from above, close HDT if it is open, and run the following

```shell
WINEPREFIX=~/.wine.hearthstone wine ~/Downloads/hdte.exe encrypt ~/Downloads/hsreplay_oauth.decrypted ~/.wine.hearthstone/drive_c/users/$USER/Application Data/HearthstoneDeckTracker/hsreplay_oauth # change ~/.wine.hearthstone to your wine prefix if you've changed it 
```

### 3. Install Hearthstone
Download hearthstone https://eu.battle.net/account/download

Run the following to install it:

```shell
WINEPREFIX=~/.wine.hearthstone wine ~/Downloads/Hearthstone-Setup.exe 
```

Select Hearthstone in Battle.net, go to Options->Game Settings, enable Additional command line arguments, and add the argument `-force-d3d9` (without this you may get a black screen on launch).

## Development

Compile using mono: `mcs hdte.cs -reference:System.Security`
