# poetry

> Manage Python packages and dependencies.
> More information: <https://python-poetry.org/docs>.

- Create a new Poetry project in the directory with a specific name:

`poetry new {{project_name}}`

- Install a dependency and its subdependencies:

`poetry add {{dependency}}`

- Install a development dependency and its subdependencies:

`poetry add --dev {{dependency}}`

- Interactively initialize the current directory as a new Poetry project:

`poetry init`

- Get the latest version of all dependencies and update `poetry.lock`:

`poetry update`

- Execute a command inside the project's virtual environment:

`poetry run {{command}}`



# Custom ...........................................................................................
```bash
poetry config --list
poetry install

poetry env info -p  # show virtualenv path

```

## Installation, Config
macOS: ~/Library/Application Support/pypoetry
Windows: C:\Users\<username>\AppData\Roaming\pypoetry
$XDG_CONFIG_HOME. That means, by default ~/.config/pypoetry.

```bash
pipx poetry
poetry completions bash > $(brew --prefix)/etc/bash_completion.d/poetry.bash-completion

poetry config virtualenvs.in-project true
vim "/Users/Q187392/Library/Application Support/pypoetry/config.toml"
```
