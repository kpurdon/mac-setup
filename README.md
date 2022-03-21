# mac-setup

This README and associated scripts are information for setting up a mac for my opinionated developer environment. I only do this once every couple years so many things here may go stale between updates. It's mostly a reference.

Last tested 03/21/2022 on the following:

```
MacBook Pro (14-inch, 2021)
Apple M1 Max
macOS Monterey (12.3)
```

## System Preferences

After the initial setup the always [install all available updates](https://support.apple.com/guide/mac-help/get-macos-updates-mchlpx1065/mac).

The following are some opinionated System Preferences.

### General

- Set the Appearance to Dark.

### Desktop & Screen Saver

- Set Hot Corners (via Screen Saver) to Bottom-Right -> Lock Screen.

### Dock & Menu Bar

- Position -> Left
- Adjust size to be smaller
- Disable magnification
- Enable Automatically Hide & Show Dock
- Battery -> Show Percentage

### Users & Groups

- Disable the Guest account

### Security & Privacy

- General
  - Allow install from App Store & Identified Developers
- FileVault -> Enable
- Firewall -> Enable
  - Block all incoming connections

### Keyboard

- Key Repeat -> Fastest
- Delay Until Repeat -> Shortest
- Modifier Keys (Repeat for all keyboards)
  - Caps Lock -> Control
  - Control -> Control
  - Option -> Option
  - Command -> Command
  - Function -> Function

### Trackpad

- Point & Click
  - Disable "Look up & data detectors"
  - Click -> Medium
  - Tracking speed -> Fast
  - Enable Force Click and haptic feedback

### Sharing

- Computer Name -> Something Useful (e.g. `kpurdon`)

### Screenshots

Run the following to put all screenshots in `~/Documents/screenshots`.

```
mkdir ~/Documents/screenshots
defaults write com.apple.screencapture location ~/Documents/screenshots
killall SystemUIServer
```

## Tools & Applications

### XCode Developer Tools

This is basically a requirement for a million things installed on macs, so install it.

`xcode-select --install`

### Chrome

[Chrome](https://www.google.com/chrome/) is my browser of choice.

### Iterm2

[Iterm2](https://iterm2.com/) is a Terminal replacement for MacOS.

#### Configuration

1. Clone [iterm2-prefs](https://github.com/kpurdon/iterm2-prefs) into `~/code/github.com/kpurdon/iterm2-prefs`
2. Open `iterm2` and select `Preferences -> Preferences -> Load Preferences (save manually)`.
3. Select `~/code/github.com/kpurdon/iterm2-prefs/` as the directory.
4. Re-start iterm2.

### SSH

- Generate a new SSH key (if `~/.ssh/id_rsa` doesn't exist)

`yes | ssh-keygen -t rsa -b 4096 -C "kylepurdon@gmail.com" -N "" -f ~/.ssh/id_rsa`

- Add the SSH key to the Keychain

`ssh-add -K ~/.ssh/id_rsa`

- Add the SSH key to GitHub

https://github.com/settings/keys

- Update your SSH config by setting `~/.ssh/config` to the following:

```
Host *
 AddKeysToAgent yes
 UseKeychain yes
 IdentityFile ~/.ssh/id_rsa
```

### Homebrew

[Homebrew](https://brew.sh/) is the package manager for MacOS (think apt-get, ...).

`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

### Rectangle

[Rectangle](https://rectangleapp.com/) is a window manager for mac. It provides keyboard shortcuts for resizing and arranging windows.

### Go

`brew install go`

### Direnv

`brew install direnv` lets you configure per-directory environment with `.envrc` files.

### Python

The easiest way to manage/install multiple Python version is using [pyenv](https://github.com/pyenv/pyenv). The [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv) pieces below are not required but highly reccomended if you use virtualenvs.

#### Before Installing Pyenv

https://github.com/pyenv/pyenv/wiki#suggested-build-environment

`brew install openssl readline sqlite3 xz zlib`

#### Installation

`brew install pyenv`

#### Pyenv Basics

```
pyenv install 3.10.3 # install a specific version
pyenv global 3.10.3  # set the global version to be used in all shells
pyenv shell 3.10.3   # set the version for the active shell
pyenv local 3.10.3   # add a .python-version file to the current directory
```

### Git

The version  of git that comes w/ MacOS is often pretty outdated. Install from brew.

```
brew install git
```

#### Configuration

```
git config --global user.name kpurdon
git config --global user.email kylepurdon@gmail.com
git config --global core.editor emacs
git config --global help.autocorect 1
git config --global pull.rebase true
git config --global init.defaultBranch main
```

#### GPG Signing

If you use commit signing, follow these instructions.

https://docs.github.com/en/github/authenticating-to-github/signing-commits
https://docs.github.com/en/github/authenticating-to-github/generating-a-new-gpg-key
https://docs.github.com/en/github/authenticating-to-github/telling-git-about-your-signing-key

1. `brew install gpg` (might need to restart after this install)
2. `gpg --full-generate-key` (rsa+rsa, 4096, no passphrase)
3. `gpg --list-secret-keys --keyid-format=long` get the id (e.g. `3AA5C34371567BD2` in the following example)
4. `pbcopy < gpg --armor --export [keyid]`
5. https://docs.github.com/en/articles/adding-a-new-gpg-key-to-your-github-account
6. `git config --global user.signingkey [keyid]` (tell git about the key)
7. `git config --global commit.gpgsign true` (tell git to always sign commits)

```
$ gpg --list-secret-keys --keyid-format=long
/Users/hubot/.gnupg/secring.gpg
------------------------------------
sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
uid                          Hubot
ssb   4096R/42B317FD4BA89E7A 2016-03-10
```

### Hack Font

[Hack](https://sourcefoundry.org/hack/) is a great mono-spaced developer font.

```
brew install homebrew/cask-fonts/font-hack
```

### Zsh (Oh-My-Zsh)

Follow the instructions in https://github.com/kpurdon/oh-my-zsh-custom to setup oh-my-zsh.

### Gitify

[Gitify](https://www.gitify.io/) is a really handy git notifications app.

### Docker

https://docs.docker.com/desktop/mac/install/

### Emacs

Install [Emacs for MacOS](https://emacsformacosx.com/) and pull down custom configuration.

```
pushd $HOME
rm -rf .emacs.d
git clone git@github.com:kpurdon/.emacs.d.git
touch custom.el
popd
```

### Zoom

https://zoom.us/download#client_4meeting

### Boxy Suite

https://www.boxysuite.com/
