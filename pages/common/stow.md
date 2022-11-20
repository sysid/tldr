# stow

> Symlink manager.
> Often used to manage dotfiles.
> More information: <https://www.gnu.org/software/stow>.

- Symlink all files recursively to a given directory:

`stow --target={{path/to/target_directory}} {{file1 directory1 file2 directory2}}`

- Delete symlinks recursively from a given directory:

`stow --delete --target={{path/to/target_directory}} {{file1 directory1 file2 directory2}}`

- Simulate to see what the result would be like:

`stow --simulate --target={{path/to/target_directory}} {{file1 directory1 file2 directory2}}`

- Delete and resymlink:

`stow --restow --target={{path/to/target_directory}} {{file1 directory1 file2 directory2}}`

- Exclude files matching a regular expression:

`stow --ignore={{regular_expression}} --target={{path/to/target_directory}} {{file1 directory1 file2 directory2}}`



# stow .............................................................................................
Symlink manager.
- package is a set of files and directories that need to be "installed" in a particular directory structure.
- The target directory is the root of the tree in which the package appear to be installed.
- When you "stow" a package, stow creates symlinks in the target directory that point into the package.

1. create a directory for every package/application
2. add the dotfiles/dotdirs into the package-dir
3. package can also contain several files or even a complex directory structure
```bash
# https://venthur.de/2021-12-19-managing-dotfiles-with-stow.html
$ pwd
/home/venthur/git/dotfiles
$ find tmux
tmux                # the package
tmux/.tmux.conf     # the dotfile

stow -t $HOME -d $HOME/dev/binx/stow tmux
stow -t $HOME -d $HOME/dev/binx/stow vim
stow -t $HOME -d $HOME/dev/binx/stow neovim

# you can stow everything at once:
stow --target=/home/venthur */
# use */ instead of * to match all directories, since my dotfiles repository also contains a README.md and a makefile

```
## Makefile
```make
# https://venthur.de/2021-12-19-managing-dotfiles-with-stow.html
# The --restow parameter tells stow to unstow the packages first before stowing them again, which is useful for pruning obsolete symlinks from the target directory.
all:
 stow --verbose --target=$$HOME --restow */

delete:
 stow --verbose --target=$$HOME --delete */
```
