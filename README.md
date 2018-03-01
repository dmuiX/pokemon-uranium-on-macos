# Pokémon Uranium on macOS

**Warning: This guide has not been fully tested on a new install of macOS. Proceed with caution!**

![](https://github.com/microbug/pokemon-uranium-on-mac/raw/master/assets/Initial%20Screenshot.png)

## Introduction
Pokémon Uranium development started around 2006 using RPG Maker XP, which as you might guess was designed for Windows XP. The game doesn’t run great on some modern computers and operating systems due to compatibility issues, and the binary is Windows-only. **But [wine](https://www.winehq.org) can run some Windows-only software, and Pokémon Uranium is supported! Wine may even run the game better than Windows installed on the same computer<sup>see appendix</sup>!** Here I’ll detail the steps you need to take to install and run Pokémon Uranium on your Mac.

This document is based off [this Reddit post](https://www.reddit.com/r/pokemonuranium/comments/6sj2rk/installing_and_playing_pokemon_uranium_with_wine/), which dealt with running Pokémon Uranium in Wine under Linux. It has been significantly expanded to help less experienced users.

### What to expect
I’ve tested Pokémon Uranium in VMWare Fusion (Windows XP VM), Wine and Bootcamp (running Windows 10 natively on my MacBook Pro). This is what I found:

#### Windows XP virtual machine:
- Almost completely unplayable, messed up audio and very laggy

#### Windows 10:
- Skips frames regularly (GPU is a Radeon Pro 455, which is more than capable enough for this)
- Has a weird menu bug where menu navigation slows to around 1 option per second (making changing controls painful)
- Has the well known line bug on a 4k display:
	- > Some people have been having problems with weird lines appearing while in fullscreen mode. I am aware of this bug and am attempting to fix it. Until then the temporary fix is to enable "Run in 640 x 480 screen resolution" in the Uranium.exe compatibility settings (This has also been known to reduce lag slightly). (<https://www.reddit.com/r/pokemonuranium/comments/525uk9/official_unofficial_patch_and_server_thread/>, 2018)

#### macOS 10.13.3 via Wine:
- Runs fairly smoothly
	- Expect a few dropped frames, but fewer than with Windows
- Doesn’t have the menu slowness bug
- Doesn’t have the line bug


## Requirements
- macOS 10.13.3+ (not tested on earlier versions, may work but no guarantees)
- Administrator privileges
- A reasonably modern Mac, the faster the better
	- I’ll keep this guide up to date with user reports, if you want to help make this guide better please see the contributing section at the end
- A vague knowledge of UNIX and the Terminal (helpful for troubleshooting but not strictly necessary if you follow this exactly and everything works)
- A copy of the current Pokémon Uranium files (the folder `C:\Program Files (x86)\Pokemon\Uranium` on a Windows install)
	- Wine cannot be used to run the installer
	- You need to download the portable version from [this Reddit post](https://www.reddit.com/r/pokemonuranium/comments/54dodq/updated_installer_patch_installers/) later on


## ① Tools
Install each tool in order.

### Homebrew
Homebrew is a package manager for macOS that can install some pieces of software.

- Open Terminal
- Paste `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"` and hit enter
- Follow instructions

### XQuartz
[XQuartz](https://www.xquartz.org) is required for Wine to run properly. To install, go to [the download page](https://www.xquartz.org) and download the most recent .dmg file, then run the .pkg file inside it and follow instructions. You don’t need to reboot yet but you will before you run the game.

###  Wine Staging
> Wine (recursive backronym for Wine Is Not an Emulator) is a free and open-source compatibility layer that aims to allow computer programs (application software and computer games) developed for Microsoft Windows to run on Unix-like operating systems. (Wikipedia, 2018)

Wine Staging is the testing area of Wine, where new features and bugs come first. At the time of writing, you must use Wine Staging to install as Wine Stable throws a fatal error.

To install Wine Staging, go to [the download page](https://dl.winehq.org/wine-builds/macosx/download.html) and download `Installer for “Wine Staging”`. Run the .pkg file once it has downloaded. Follow instructions. During installation, tick the `64 bit support (optional)` box.

### Winetricks
> Winetricks is a helper script to download and install various redistributable runtime libraries needed to run some programs in Wine. These may include replacements for components of Wine using closed source libraries. (WineHQ Wiki, 2018)

Now that you have Homebrew installed, open Terminal and type `brew install winetricks`, then press enter and wait for it to finish installing. That’s it!

## ② Game Installation
### Restart
Before continuing, restart to allow your Mac to use the newly installed XQuartz software.

### Create virtual Windows installation
Run the following commands in order (you can copy and paste the whole block). You will be able to move the `~/pokemon_uranium` folder (which will contain the game and Wine configuration) to your preferred location once everything is ready. Lots of warnings/errors will be created, don’t worry about these.

```bash
# make game folder
mkdir ~/pokemon_uranium
# set WINEPREFIX to point to folder
export WINEPREFIX=~/pokemon_uranium
# enter folder
cd $WINEPREFIX
# run wineboot
wineboot
# install DLLs
winetricks directplay directmusic dsound d3dx9_36 devenum dmsynth quartz
```

### Add the game to the virtual Windows installation
Go to [this Reddit post](https://www.reddit.com/r/pokemonuranium/comments/54dodq/updated_installer_patch_installers/) and click on the **Portable** link to download the game files. The installer won’t work so it needs to be the portable version! Extract the zip file once it has downloaded by double clicking on it.

Alternatively, copy from an existing Windows installation the folder `C:\Program Files (x86)\Pokemon Uranium`.

In Finder press SHIFT+CMD+G and type `~/pokemon_uranium/drive_c/Program Files (x86)` into the box, then hit enter. Move the `Pokemon Uranium` folder from either the extracted archive or the existing installation to the folder you are now in.

### Run the game once to install fonts
Download the `Run Pokémon Uranium.command` file from this Git repository and run `chmod u+x ~/Downloads/Run Pokémon Uranium.command` in the Terminal. Move the file to `~/pokemon_uranium`. Double click the file to start Pokémon Uranium.

The fonts will look a bit weird on this first run. The game will automatically install the fonts it needs at this point. Press enter to accept when the game asks you if you want to restart it. It will close (and doesn’t automatically restart).

### Move the game to your preferred folder
You can leave the game in `~/pokemon_uranium`, but if you want to move it to (for example) `~/Documents/Pokémon Uranium` you can now do so through Finder.

## ③ Play the game 😄👏
Every time you want to play, double click `Run Pokémon Uranium.command` in your game directory to start it. You can also drag `Run Pokémon Uranium.command` to the right side of your Dock (next to the Trash). Your saves are in `<game directory>/drive_c/users/<username>/Saved Games/Pokemon Uranium`.

## Contributing
### System requirements
If you’d like to contribute to this guide, the easiest thing you can do is to send me your computer’s specs and let me know how well Pokémon Uranium runs for you. This will help create a more accurate requirements section.

If you know how, you can create a pull request that adds a line to `Experiences.csv` (check the headers of that file for what to add where).

Otherwise, please send an email to <richard@microbug.uk>. Please use the following template: 

```
Does Pokémon Uranium run at all under Wine on your Mac?
[...]

If yes, on a scale of 1 to 10, where 1 is barely and 10 is flawlessly,
how well does Pokémon Uranium run on your Mac?
[...]

Did you encounter any specific bugs? The more detail the better!
[...]

Remember to attach your screenshot or this is useless!

Thank you for contributing to this project
```

Please attach a screenshot of your ‘About this Mac’ page (you can black out the serial number and UUID with Preview if you wish, I won’t publish and don’t need that information). You can do this as follows:

- Click on the Apple menu (at the very top left of your screen).
- Select `About this Mac`
- Take a screenshot using the built in Grab app (the image is saved to the Desktop)
- Optionally remove/cover your serial number and hardware UUID
- **Attach to email**

### Other contributions
If you know what you’re doing or have a suggestion, please make a pull request or open an issue on GitHub. You can also email me at <richard@microbug.uk>. My PGP key is available at <https://keybase.io/microbug>.

