# asciinema

> Record and replay terminal sessions, and optionally share them on <https://asciinema.org>.
> See also: `terminalizer`.
> More information: <https://docs.asciinema.org/manual/cli/usage>.

- Associate the local install of `asciinema` with an asciinema.org account:

`asciinema auth`

- Make a new recording (finish it with `Ctrl+D` or type `exit`, and then choose to upload it or save it locally):

`asciinema rec`

- Make a new recording and save it to a local file:

`asciinema rec {{path/to/recording.cast}}`

- Replay a terminal recording from a local file:

`asciinema play {{path/to/recording.cast}}`

- Replay a terminal recording hosted on <https://asciinema.org>:

`asciinema play https://asciinema.org/a/{{cast_id}}`

- Make a new recording, limiting any idle time to at most 2.5 seconds:

`asciinema rec {{-i|--idle-time-limit}} 2.5`

- Print the full output of a locally saved recording:

`asciinema cat {{path/to/recording.cast}}`

- Upload a locally saved terminal session to asciinema.org:

<<<<<<< HEAD
<<<<<<< HEAD
`asciinema upload {{path/to/file}}.cast`


# Operations .......................................................................................
[Sharing & embedding - asciinema](https://asciinema.org/docs/embedding)

- to record tmux:
- setup your tmux session (tmux new -s session-name, create windows, splits, start processes in them)
- detach (prefix+d)
- run asciinema rec -c "tmux attach -t session-name"
- when you're finished, just detach the session again

```bash
# setup session
# detach (prefix+d)
asciinema rec -c "tmux attach"
# detach (prefix+d)
asciinema auth
asciinema upload <file>

# including in rst as link (Github) or embedded
.. raw:: html

   <a href="https://asciinema.org/a/278453?t=0.2" target="_blank"><img src="https://asciinema.org/a/278453.svg" /></a>

.. raw:: html

    <script id="asciicast-278453" src="https://asciinema.org/a/278453.js" async></script>
```
=======
`asciinema upload {{path/to/file.cast}}`
>>>>>>> e7d5609dd3df37e241ace0e204d8393fdbd0de49
=======
`asciinema upload {{path/to/recording.cast}}`
>>>>>>> 2624ea6a30f5c9afdc2bc12c9118f080d0bff6d4
