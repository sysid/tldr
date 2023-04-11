# pre-commit

> Create Git hooks that get run before a commit.
> More information: <https://pre-commit.com>.

- Install pre-commit into your Git hooks:

`pre-commit install`

- Run pre-commit hooks on all staged files:

`pre-commit run`

- Run pre-commit hooks on all files, staged or unstaged:

`pre-commit run --all-files`

- Clean pre-commit cache:

`pre-commit clean`


# Custom ...........................................................................................
[pre-commit](https://pre-commit.com/#creating-new-hooks)
```bash
# dry-run a hook
pre-commit
pre-commit run decrypt-env-file --verbose --hook-stage post-checkout

# install AND upgrade python version
pre-commit install
```

## post-commit
pre-commit framework does not automatically run hooks in the post-checkout stage when switching branches.
The pre-commit command is primarily used to run hooks in the pre-commit stage.
The post-checkout stage is not triggered by the pre-commit command itself but rather by Git when checking out a branch.

To run hooks in the post-checkout stage, you need to set up a separate Git hook for that stage.
1. create pre-commit config:
```yaml
# .pre-commit-config.yaml
repos:
- repo: local
  hooks:
  - id: decrypt-env-file
    name: Decrypt environment file
    entry: sh -c "echo 'Hallo ...............'"
    language: system
    files: http-client.private.env.enc.json
    always_run: true
    verbose: true  # prevents output from being swallowed
    stages:
    - post-checkout
    - post-commit
```

2. Create a new executable file named post-checkout (no file extension) in the .git/hooks directory of your repository.
3. Edit the post-checkout file to run the pre-commit command for the post-checkout stage. Add the following content to the file:
```bash
#!/bin/sh
pre-commit run decrypt-env-file --hook-stage post-checkout

# test dry-run
pre-commit run decrypt-env-file --verbose --hook-stage post-checkout
```
Now, when you switch branches, the post-checkout Git hook will run, and it will execute the decrypt-env-file hook in the post-checkout stage.



## Gotcha
### Output of hooks
- is silent unless there is a problem (by default).
- if script fails then the output will be displayed
- you can also use --verbose to show the full output (for debugging) and optionally set verbose: true on your hook itself.
```bash
pre-commit run decrypt-env-file --verbose --hook-stage post-checkout
```

## Example
```yaml
fail_fast: true
repos:
- repo: local
  hooks:
  - id: custom-check
    name: Custom Check
    entry: scripts/custom-check.sh
    language: script
    files: \.py$
    verbose: true  # prevents output from being swallowed

  - id: black
    name: black
    stages: [commit]
    language: system
    entry: pipenv run black
    types: [python]
    exclude: setup.py

  - id: flake8
    name: flake8
    stages: [commit]
    language: system
    entry: pipenv run flake8
    types: [python]
    exclude: setup.py

  - id: isort
    name: isort
    stages: [commit]
    language: system
    entry: pipenv run isort
    types: [python]

  - id: black
    name: black
    stages: [commit]
    language: system
    entry: pipenv run black
    types: [python]

  - id: flake8
    name: flake8
    stages: [commit]
    language: system
    entry: pipenv run flake8
    types: [python]
    exclude: setup.py

  - id: mypy
    name: mypy
    stages: [commit]
    language: system
    entry: pipenv run mypy
    types: [python]
    require_serial: true

  - id: pytest
    name: pytest
    stages: [commit]
    language: system
    entry: pipenv run pytest
    types: [python]
    pass_filenames: false

  - id: pytest-cov
    name: pytest
    stages: [push]
    language: system
    entry: pipenv run pytest --cov --cov-fail-under=100
    types: [python]
    pass_filenames: false

  - id: terraform-fmt
    name: terraform-fmt
    stages: [commit]
    language: system
    entry: sh -c 'for file do terraform fmt $file; done' --
    types: [terraform]
    verbose: true
```
