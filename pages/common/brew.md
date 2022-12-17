# brew

> Package manager for macOS and Linux.
> More information: <https://brew.sh>.

- Install the latest stable version of a formula or cask (use `--devel` for development versions):

`brew install {{formula}}`

- List all installed formulae and casks:

`brew list`

- Upgrade an installed formula or cask (if none is given, all installed formulae/casks are upgraded):

`brew upgrade {{formula}}`

- Fetch the newest version of Homebrew and of all formulae and casks from the Homebrew source repository:

`brew update`

- Show formulae and casks that have a more recent version available:

`brew outdated`

- Search for available formulae (i.e. packages) and casks (i.e. native packages):

`brew search {{text}}`

- Display information about a formula or a cask (version, installation path, dependencies, etc.):

`brew info {{formula}}`

- Check the local Homebrew installation for potential problems:

`brew doctor`


# Custom ....................................................................................................
[The Missing Package Manager for macOS (or Linux) — Homebrew](https://brew.sh/)

## Bundles
[📦 Bundler for non-Ruby dependencies from Homebrew, Homebrew Cask and the Mac App Store.](https://github.com/Homebrew/homebrew-bundle#usage)
```bash
# create your onw `Brewfile` for custom installation:
brew bundle dump
# cleanup
brew bundle cleanup --force
```

## Cask
You can find custom commands for each application amongst available Casks, but generally, brew cask install just retrieves the configured version of the executable file and moves it to the specified application directory (~/Applications by default).
$BREW_DIR/Caskroom contains the list of casks installed, and .metadata folder in each cask mentions the cask file used during installation.
The app directory you see in ~/Applications is not a symlink. If it is so, check that app's cask file and it should contain clues to the real location.
```bash
# if you want to know where exactly a formula has been installed you can use the command info
brew info --cask <formula>
brew info --cask intellij-idea  # shows formula source code link, e.g. destination paths
brew install --cask keepassxc
```


# Tools
[GitHub - whalebrew/whalebrew: Homebrew, but with Docker images](https://github.com/whalebrew/whalebrew)
[GitHub - mas-cli/mas: Mac App Store command line interface](https://github.com/mas-cli/mas)
[How To Install and Use Homebrew on Linux | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-homebrew-on-linux)
