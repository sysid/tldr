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
- message "(no files to check)" for a specific hook, it means that the hook has been executed, but no files matched the criteria defined for that hook
- hook only runs on files that have been staged for a commit (default)
```bash
# Run a specific hook on all files:
pre-commit run hook-id --all-files

# Run all hooks on a specific file:
pre-commit run --files path/to/your/file

# Run a specific hook on a specific file:
pre-commit run hook-id --files path/to/your/file

# dry-run a hook
pre-commit
pre-commit run decrypt-env-file --verbose --hook-stage post-checkout

# install AND upgrade python version
pre-commit install

# test it
pre-commit run --all-files
pre-commit run --files YOUR_FILENAME
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
- staging of files within pre-commit: must be in same process (!?)

### Output of hooks
- is silent unless there is a problem (by default).
- if script fails then the output will be displayed
- you can also use --verbose to show the full output (for debugging) and optionally set verbose: true on your hook itself.
```bash
pre-commit run decrypt-env-file --verbose --hook-stage post-checkout
```
### pyprojet
[Allow pyproject.toml to define pre-commit config · Issue #1165 · pre-commit/pre-commit · GitHub](https://github.com/pre-commit/pre-commit/issues/1165)
- not planned!!


## Development/Testing
```bash
# To test your hooks on specific files or all files in your repository, you can use one of the following commands:
# To run the hooks on a specific file or files:
pre-commit run --files path/to/file1.py path/to/file2.py
# To run the hooks on all files in the repository:
pre-commit run --all-files
```


## Example
```yaml
# .pre-commit-config.yaml
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
    require_serial: true  # !!!!

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
    pass_filenames: false  # run without any arguments, so it won't fail even if some of the staged files would cause the hook to fail

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
