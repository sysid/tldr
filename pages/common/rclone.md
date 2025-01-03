# rclone

> Copy, synchronize or move files and directories to and from many cloud services.
> More information: <https://rclone.org>.

- Launch an interactive menu to setup rclone:

`rclone config`

- List contents of a directory on an rclone remote:

`rclone lsf {{remote_name}}:{{path/to/directory}}`

- Copy a file or directory from the local machine to the remote destination:

`rclone copy {{path/to/source_file_or_directory}} {{remote_name}}:{{path/to/directory}}`

- Copy files changed within the past 24 hours to a remote from the local machine, asking the user to confirm each file:

`rclone copy --interactive --max-age 24h {{remote_name}}:{{path/to/directory}} {{path/to/local_directory}}`

- Mirror a specific file or directory (Note: Unlike copy, sync removes files from the remote if it does not exist locally):

`rclone sync {{path/to/file_or_directory}} {{remote_name}}:{{path/to/directory}}`

- Delete a remote file or directory (Note: `--dry-run` means test, remove it from the command to actually delete):

`rclone --dry-run delete {{remote_name}}:{{path/to/file_or_directory}}`

- Mount rclone remote (experimental):

`rclone mount {{remote_name}}:{{path/to/directory}} {{path/to/mount_point}}`

- Unmount rclone remote if CTRL-C fails (experimental):

`fusermount -u {{path/to/mount_point}}`


# Custom ...........................................................................................
- alternative: S3 sync
```bash
rclone sync . tw-prod-bkp:/tw-prod-bkp/mac/data --progress
```

## Config
```bash
○ → more /Users/Q187392/.config/rclone/rclone.conf
───────┬───────────────────────────────────────────────────────────────────────────────────────────
       │ File: /Users/Q187392/.config/rclone/rclone.conf
───────┼───────────────────────────────────────────────────────────────────────────────────────────
   1   │ [tw-prod-bkp]
   2   │ type = s3
   3   │ provider = AWS
   4   │ env_auth = true
   5   │ region = eu-central-1
   6   │ server_side_encryption = AES256
───────┴───────────────────────────────────────────────────────────────────────────────────────────

```
## ,hk
```bash
# rclone-data {{{ #
rclone-data () {  ## sync directories to AWS
  twpushd ~/data
  AWS_PROFILE=tw-admin rclone sync . tw-prod-bkp:/tw-prod-bkp/mac/data --progress
  twpopd
}
# }}} rclone #
```

