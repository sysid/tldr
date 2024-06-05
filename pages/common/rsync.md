# rsync

> Transfer files either to or from a remote host (but not between two remote hosts), by default using SSH.
> To specify a remote path, use `user@host:path/to/file_or_directory`.
> More information: <https://download.samba.org/pub/rsync/rsync.1>.

- Transfer a file:

`rsync {{path/to/source}} {{path/to/destination}}`

- Use archive mode (recursively copy directories, copy symlinks without resolving, and preserve permissions, ownership and modification times):

`rsync {{-a|--archive}} {{path/to/source}} {{path/to/destination}}`

- Transfer file in [a]rchive (to preserve attributes) and compressed ([z]ipped) mode with [v]erbose and [h]uman-readable [P]rogress/partial:

`rsync {{-zvhP|--compress --verbose --human-readable --partial --progress}} {{path/to/source}} {{path/to/destination}}`

- Recursively copy directories:

`rsync {{-r|--recursive}} {{path/to/source}} {{path/to/destination}}`

- Transfer directory contents, but not the directory itself:

`rsync {{-r|--recursive}} {{path/to/source}}/ {{path/to/destination}}`

- Use archive mode, resolve symlinks, and skip files that are newer on the destination:

`rsync {{-auL|--archive --update --copy-links}} {{path/to/source}} {{path/to/destination}}`

- Transfer a directory from a remote host running `rsyncd` and delete files on the destination that do not exist on the source:

`rsync {{-r|--recursive}} --delete rsync://{{host}}:{{path/to/source}} {{path/to/destination}}`

- Transfer a file over SSH using a different port than the default (22) and show global progress:

`rsync {{-e|--rsh}} 'ssh -p {{port}}' --info=progress2 {{host}}:{{path/to/source}} {{path/to/destination}}`


# Custom ...........................................................................................
- exlcude all but one file-type
  When using multiple include/exclude option, the first matching rule applies.


`rsync -a -m --include='*.jpg' --include='*/' --exclude='*' src_directory/ dst_directory/`
--include='*.jpg' : - First we are including all .jpg files.
--include='*/'    : - Then we are including all directories inside the in src_directory directory. Without this rsync will only copy '*.jpg' files in the top level directory.
-m                : - Removes the empty directories
