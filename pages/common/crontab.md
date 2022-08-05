# crontab

> Schedule cron jobs to run on a time interval for the current user.
> More information: <https://crontab.guru/>.

- Edit the crontab file for the current user:

`crontab -e`

- Edit the crontab file for a specific user:

`sudo crontab -e -u {{user}}`

- Replace the current crontab with the contents of the given file:

`crontab {{path/to/file}}`

- View a list of existing cron jobs for current user:

`crontab -l`

- Remove all cron jobs for the current user:

`crontab -r`

- Sample job which runs at 10:00 every day (* means any value):

`0 10 * * * {{command_to_execute}}`

- Sample crontab entry, which runs a command every 10 minutes:

`*/10 * * * * {{command_to_execute}}`

- Sample crontab entry, which runs a certain script at 02:30 every Friday:

`30 2 * * Fri {{/absolute/path/to/script.sh}}`


The job defined by this string runs at startup, immediately after Linux reboots.
`@reboot /usr/bin/crontab -l > $HOME/dev/binx/crontab/crontab.$(uname -n).bkp 2>&1`

```bash
# Crontab Example
# CAVEAT: machine is on UTC
#
SHELL=/bin/bash
PROJ_DIR=/home/e4madm/powerpool
PATH=/home/e4madm/.local/bin:/home/e4madm/.asdf/shims:/home/e4madm/.asdf/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/e4madm/.asdf/installs/python/3.9.4/bin:/home/e4madm/.asdf/installs/python/3.9.4/bin:/home/e4madm/.local/bin:/home/e4madm/.local/bin
#PATH=$PATH:$HOME/.asdf/shims
# Powerpool Cron jobs
#30 * * * * /home/e4madm/rsync_powerpool_tests.sh >> /home/e4madm/rsync.log 2>&1

0 17 * * * (cd $PROJ_DIR && direnv exec . bash -l -c /home/e4madm/powerpool/service/simulation/scripts/sim-regression-run.sh) > /home/e4madm/sim-regression-run.log 2>&1
#* * * * * (cd $PROJ_DIR && direnv exec . bash -l -c 'pipenv') > /home/e4madm/sim-regression-run.log 2>&1
* * * * * /usr/bin/env > $HOME/cron-env.out
0 5 * * * /home/e4madm/docker_prune.sh
```
