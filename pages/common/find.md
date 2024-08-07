# find

> Find files or directories under a directory tree, recursively.
> More information: <https://manned.org/find>.

- Find files by extension:

`find {{root_path}} -name '{{*.ext}}'`

- Find files matching multiple path/name patterns:

`find {{root_path}} -path '{{**/path/**/*.ext}}' -or -name '{{*pattern*}}'`

- Find directories matching a given name, in case-insensitive mode:

`find {{root_path}} -type d -iname '{{*lib*}}'`

- Find files matching a given pattern, excluding specific paths:

`find {{root_path}} -name '{{*.py}}' -not -path '{{*/site-packages/*}}'`

- Find files matching a given size range, limiting the recursive depth to "1":

`find {{root_path}} -maxdepth 1 -size {{+500k}} -size {{-10M}}`

- Run a command for each file (use `{}` within the command to access the filename):

`find {{root_path}} -name '{{*.ext}}' -exec {{wc -l}} {} \;`

- Find all files modified today and pass the results to a single command as arguments:

`find {{root_path}} -daystart -mtime {{-1}} -exec {{tar -cvf archive.tar}} {} \+`

- Find empty files (0 byte) or directories and delete them verbosely:

<<<<<<< HEAD
`find {{root_path}} -type {{f}} -empty -delete`


# Custom  ..........................................................................................
```bash
# remove dot slash of relative path
find -type f -printf '%P\n'

find /path/to/files/ -type f -name '*.md' -mtime +30 -size -4M -not -path './tmp/*' -not! -path './.venv/*' -print0 | xargs -0 printf '%s\n'

# exclude paths
find . -name '.envrc' \
    -not -path './crypt/*' \
    -not -path './.venv/*' \
    -type f \
    -printf '%P' | gxargs --no-run-if-empty --verbose -n 1 -d $'\n' -- $0 _cp_one "$BASEPATH"

# replace links with files
find ./ -not -path './.venv/*' -not -path './.git/*' -type l -print0|xargs -0 -i sh -c 'cp --remove-destination  $(readlink "{}") "{}" '

# find broken links
find /target/dir -type l ! -exec test -e {} \; -print

# chain commands
find /target/dir -type l ! -exec test -e {} \; -exec rm {} \;

# reverse find links associated with file (inode)
find -L /Users/q187392/dev/s/public/ -samefile ref-file

# delete line recursively: sed edit direct without backup
find . -type f -name ".envrc" -exec sed -i '/source ~\/dev\/crypt\/dot_direnv_creds/d' {} \;
```
||||||| b8fa25ce4
`find {{root_path}} -type {{f}} -empty -delete`
=======
`find {{root_path}} -type {{f|d}} -empty -delete -print`
>>>>>>> d18350312d609b0e40301bf2b6316238204d21ec
