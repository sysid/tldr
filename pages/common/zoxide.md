# zoxide

> Keep track of the most frequently used directories.
> Uses a ranking algorithm to navigate to the best match.
> More information: <https://github.com/ajeetdsouza/zoxide>.

- Go to the highest-ranked directory that contains "foo" in the name:

`zoxide query {{foo}}`

- Go to the highest-ranked directory that contains "foo" and then "bar":

`zoxide query {{foo}} {{bar}}`

- Start an interactive directory search (requires `fzf`):

`zoxide query --interactive`

- Add a directory or increment its rank:

`zoxide add {{path/to/directory}}`

- Remove a directory from `zoxide`'s database interactively:

`zoxide remove {{path/to/directory}} --interactive`

- Generate shell configuration for command aliases (`z`, `za`, `zi`, `zq`, `zr`):

`zoxide init {{bash|fish|zsh}}`


# Custom ...........................................................................................

---
<!--ID:1700326374777-->
1. How to use zoxide?
> ```bash
> z foo              # cd into highest ranked directory matching foo
> z foo /            # cd into a subdirectory starting with foo
>
> z ~/foo            # z also works like a regular cd command
> z foo/             # cd into relative path
>
> zi foo             # cd with interactive selection (using fzf)
>
> z foo<SPACE><TAB>  # show interactive completions (zoxide v0.8.0+, bash 4.4+/fish/zsh only)
> ```

---

