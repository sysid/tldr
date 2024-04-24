# fzf

> Command-line fuzzy finder.
> Similar to `sk`.
> More information: <https://github.com/junegunn/fzf>.

- Start `fzf` on all files in the specified directory:

`find {{path/to/directory}} -type f | fzf`

- Start `fzf` for running processes:

`ps aux | fzf`

- Select multiple files with `Shift + Tab` and write to a file:

`find {{path/to/directory}} -type f | fzf --multi > {{path/to/file}}`

- Start `fzf` with a specified query:

`fzf --query "{{query}}"`

- Start `fzf` on entries that start with core and end with either go, rb, or py:

`fzf --query "^core go$ | rb$ | py$"`

- Start `fzf` on entries that not match pyc and match exactly travis:

`fzf --query "!pyc 'travis"`


# Custom ...........................................................................................
- [fzf shell key bindings](https://github.com/junegunn/fzf/wiki/Configuring-shell-key-bindings)
- uses `find` per default

CTRL-J/K        move cursor
CTRL-I          select multi
ESC, CTRL-C     end
CTRL-T          paste filename to commandline (!!)
CTRL-R          command history

CTRL-E          edit vim
CTRL-O          open

- bm            Google bookmarsk

## vim
<C-V>           vertical window
<C-T>           new tab

:Colors
:Commits
:Commands
:GFiles
:Tags
:Maps
:Filetypes

- vimwiki tag search: `:FzfTagQuery <tag> & td`

## Installation/Configuration
https://github.com/junegunn/fzf#installation: brew or git
- upgrade: `brew upgrade fzf`
```sh
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install

# .bashrc
if type rg &> /dev/null; then
  export FZF_DEFAULT_COMMAND='rg --files'
  export FZF_DEFAULT_OPTS='-m --height 50% --border'
fi
```

