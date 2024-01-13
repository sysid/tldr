# poetry

> Manage Python packages and dependencies.
> See also: `asdf`.
> More information: <https://python-poetry.org/docs/cli/>.

- Create a new Poetry project in the directory with a specific name:

`poetry new {{project_name}}`

- Install and add a dependency and its sub-dependencies to the `pyproject.toml` file in the current directory:

`poetry add {{dependency}}`

- Install the project dependencies using the `pyproject.toml` file in the current directory:

`poetry install`

- Interactively initialize the current directory as a new Poetry project:

`poetry init`

- Get the latest version of all dependencies and update `poetry.lock`:

`poetry update`

- Execute a command inside the project's virtual environment:

`poetry run {{command}}`

<<<<<<< HEAD


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
=======
- Bump the minor version of the project in `pyproject.toml`:

`poetry version minor`
<<<<<<< HEAD

- List all poetry subcommands:

`poetry list`
>>>>>>> 59e59ea818f8c7c227993532923f450b3c45701d
=======
>>>>>>> 525756888790a9c7d4e41ac21f324afb3f6f1475
