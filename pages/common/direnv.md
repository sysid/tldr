# direnv

> Shell extension to load and unload environment variables depending on the current directory.
> More information: <https://github.com/direnv/direnv>.

- Grant direnv permission to load the `.envrc` present in the current directory:

`direnv allow {{.}}`

- Revoke the authorization to load the `.envrc` present in the current directory:

`direnv deny {{.}}`

- Edit the `.envrc` file in the default text editor and reload the environment on exit:

`direnv edit {{.}}`

- Trigger a reload of the environment:

`direnv reload`

- Print some debug status information:

`direnv status`


# Custom ...........................................................................................
- alias not working in `.envrc`
- `dotenv .env.local` triggers resourcing after change, must not contain anything expect `export`
- gotcha: `dotenv`: not working when called twice within same `.envrc`
  Consequence: source manually, do not use `dotenv`

```bash
direnv show_dump $(direnv dump)  # show dump (inner bash state at the end of execution)
```

[Python · direnv/direnv Wiki · GitHub](https://github.com/direnv/direnv/wiki/Python)
- python: https://rnorth.org/practical-direnv

```bash
# non-interactive:
direnv allow . && eval "$(direnv export bash)"
```

## Configuration
global `.direnvrc` which essentially gets sourced by every local `.envrc`

## nesting
- does not merge, but reset
- workaround: add `source_env ..` or `source_up` to the file so it sources the parent folder

## Convention
- put `alias` in `script/env.sh` and source it via `$env`


## Creating functions and aliases
/Users/q187392/dev/binx/dotfile/dot.direnvrc

### Gotcha
# without direnv
alias greet="echo hello"

# with direnv
export_alias greet 'echo hello $@'


## Gotcha
- use `dotenv` instead of `source` for proper unloading, but source seems to work ?!?



# Concept, Working .................................................................................
- when direnv is invoked, it is run as a subprocess, much like any other command-line program.
- When it is run, it does not directly modify the environment of the shell from which it was invoked.
- it cleverly outputs shell commands that are immediately evaluated by the shell, thanks to the direnv hook that was installed when you set up direnv.
```bash
eval "$(direnv hook bash)"


_direnv_hook() {
  local previous_exit_status=$?;
  trap -- '' SIGINT;
  eval "$("/opt/homebrew/bin/direnv" export bash)";
  trap - SIGINT;
  return $previous_exit_status;
};
if ! [[ "${PROMPT_COMMAND:-}" =~ _direnv_hook ]]; then
  PROMPT_COMMAND="_direnv_hook${PROMPT_COMMAND:+;$PROMPT_COMMAND}"
fi
```
the `eval "$( ... )"` part tells the shell to evaluate that snippet, effectively adding direnv's functionality as part of the shell itself.

- Now, every time you cd into a directory or otherwise change the shell environment, the direnv hook that was installed into your shell gets executed.
- The hook invokes direnv, which evaluates the necessary environment changes and outputs the appropriate shell commands.
- The hook then immediately evaluates those commands, thus changing the shell's environment as needed.


# Explanation of the `direnv` Hook Script .........................................................
## `_direnv_hook` Function Definition
```bash
_direnv_hook() {
  local previous_exit_status=$?;
  trap -- '' SIGINT;
  eval "$("/opt/homebrew/bin/direnv" export bash)";
  trap - SIGINT;
  return $previous_exit_status;
};
```

### Explanation:

- `local previous_exit_status=$?`:  
  Saves the current exit status of the last command run before this function is invoked.

- `trap -- '' SIGINT`:  
  Sets up a trap for the `SIGINT` signal (sent to the shell when you press `Ctrl-C`) and ignores it to prevent interruption while `direnv` is updating the environment.

- `eval "$("/opt/homebrew/bin/direnv" export bash)"`:  
  Runs `direnv export bash` which outputs a series of shell commands to update the shell session's environment based on the `.envrc` file in the current directory. `eval` then executes these commands, thus updating the environment.

- `trap - SIGINT`:  
  Restores the normal `SIGINT` behavior, which was temporarily overridden earlier in the function.

- `return $previous_exit_status`:  
  Ensures that the function exits with the same status as was present when the function was invoked.


## Adding `_direnv_hook` to `PROMPT_COMMAND`
```bash
if ! [[ "${PROMPT_COMMAND:-}" =~ _direnv_hook ]]; then
  PROMPT_COMMAND="_direnv_hook${PROMPT_COMMAND:+;$PROMPT_COMMAND}"
fi
```

### Explanation:

- `if ! [[ "${PROMPT_COMMAND:-}" =~ _direnv_hook ]]; then`:  
  Checks whether `_direnv_hook` is already part of the `PROMPT_COMMAND`.

- `PROMPT_COMMAND="_direnv_hook${PROMPT_COMMAND:+;$PROMPT_COMMAND}"`:  
  Sets the `PROMPT_COMMAND` variable to first run `_direnv_hook`, and then whatever was in `PROMPT_COMMAND` before. If `PROMPT_COMMAND` was previously unset or empty, it sets `PROMPT_COMMAND` to `_direnv_hook`.

# Summary

In summary, this hook script defines a function `_direnv_hook` that asks `direnv` how the environment should be modified and then applies those modifications.
It then arranges for this function to be run every time the shell prompt is about to be displayed by adding it to `PROMPT_COMMAND`.
This ensures that `direnv` gets a chance to update the environment every time you run a command.

