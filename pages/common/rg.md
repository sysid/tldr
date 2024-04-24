# rg

> Ripgrep is a recursive line-oriented search tool.
> Aims to be a faster alternative to `grep`.
> More information: <https://github.com/BurntSushi/ripgrep>.

- Recursively search the current directory for a regular expression:

`rg {{regular_expression}}`

- Search for regular expressions recursively in the current directory, including hidden files and files listed in `.gitignore`:

`rg --no-ignore --hidden {{regular_expression}}`

- Search for a regular expression only in a subset of directories:

`rg {{regular_expression}} {{set_of_subdirs}}`

- Search for a regular expression in files matching a glob (e.g. `README.*`):

`rg {{regular_expression}} --glob {{glob}}`

- Search for filenames that match a regular expression:

`rg --files | rg {{regular_expression}}`

- Only list matched files (useful when piping to other commands):

`rg --files-with-matches {{regular_expression}}`

- Show lines that do not match the given regular expression:

`rg --invert-match {{regular_expression}}`

- Search a literal string pattern:

`rg --fixed-strings -- {{string}}`


# Custom ..........................................................................................
https://github.com/BurntSushi/ripgrep

ag, ripgrep, grep

rg -uuu                 search all ()== grep -a -r)
rg -i                   ignore case
rg -v                   invert
rg -C2                  2 lines before/after

rg foo -g '*.html'      glob files
rg foo -g '!*.html'     inverse glob files

rg --type-list          list filetypes
rg foo -thtml           search filetype
rg foo -Thtml           inverse search filetype
```sh
rg --type-list
rg -tsh
# sort after modified data
rg --column --line-number --no-heading --color=always --smart-case --sortr modified -- '^- \[[ ]\] .+'

# grep todos
rg --column --line-number --no-heading --color=always --smart-case --sortr modified -- '^- \[[ ]\] .+' ~/vimwiki | egrep -v '{noshow}|complete' | sort -k4 -r

# grep two tags with AND
rg ':xxx[:]{0,1}.*e4m:|:e4m[:]{0,1}.*xxx:'
```

## Gotcha
use `-uu` to include hidden files and ignore `.gitignore` (e.g. pyo-pool)



# [locate](/help/ubuntu.md#search), [mdfind](/help/macos.md#search), [find](/help/bash.md#search)


