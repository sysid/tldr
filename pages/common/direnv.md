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
