# Setup poetry

This is a composite action which handles installing python and poetry tool, using which it then installs your project's
poetry dependencies.

The main goal of this action is to **quickly** handle installing of the project's dependencies, by effectively
utilizing caching. While there are some other actions with the same goal, most of them don't have good (if any)
cross-platform support, and don't focus as much on speed.

NOTE: Windows support may be a bit flaky due to some custom complex logic to allow it's caching. If you do see bugs,
please report them.

## Usage

### Example

You can use this action as follows:

```yaml
- name: Install Python Dependencies
  uses: ItsDrike/setup-poetry@v1
  with:
    install-args: "--without dev"
    python-version: '3.10'
    poetry-version: '1.3.1'
```

### Inputs

The following inputs can be passed to
| Name                     | Type   | Default                   | Description                                                                                                           |
|--------------------------|--------|---------------------------|-----------------------------------------------------------------------------------------------------------------------|
| python-version           | string |                           | The version of python to use (passed into `actions/setup-python` action).                                             |
| poetry-version           | string |                           | Poetry version to use (passed as `--version` argument to the [official poetry install script][poetry-installer-docs]. |
| install-args             | string |                           | A string placed after the `poetry install` command, which can contain extra options.                                  |
| working-dir              | path   | `.`                       | The directory to run the `poetry` commands in. By default, this will just be the root directory of the project.       |
| cache-poetry-install     | bool   | true                      | Enable caching for poetry tool itself (not related to caching of the project's poetry dependencies).                  |
| cache-poetry-environment | bool   | true                      | Enable caching for poetry environments (venvs that contain the project's poetry dependencies).                        |
| poetry-home              | path   | `~/.local/share/pypoetry` | Directory to install the poetry tool into. (passed into the official poetry installer as `POETRY_HOME` env variable). |


### Outputs

The following outputs are produced by the action:

| Name                         | Type   | Description                                                                    |
|------------------------------|--------|--------------------------------------------------------------------------------|
| poetry-venv-path             | path   | Path to the virtual environment created by poetry for the project.             |
| python-version               | string | The python version used (forwarded from `actions/setup-python` action).        |
| python-path                  | path   | Path to the python interpreter (forwarded from `actions/setup-python` action). |
| cache-hit-poetry-install     | bool   | The was a cache hit for the poetry tool (installation) cache.                  |
| cache-hit-poetry-environment | bool   | There was a cache hit for the poetry environment (dependencies) cache.         |


[poetry-installer-docs]: https://python-poetry.org/docs/#installing-with-the-official-installer
