---
title: "Poetry"
date: 2023-12-02T16:38:42+04:00
draft: true
---

## Why poetry?

I'm new to Python, but I think that `poetry` simplifying the process of setting up venv and building.

## How to use this snazzy stuff?

A `pyproject.toml` file contains build system requirements and information, which are used by pip(or poetry) to build the package.

You need to install `poetry` by `pip`.

And write:

```bash
poetry init
```

Installing a package:

```bash
poetry add PACKAGE
```

Updating(it also updates `poetry.lock`):

```bash
poetry update
```

Running a script

```bash
poetry run python SCRIPT.py
```

### Specifying dependencies

Dependencies are placed in a `pyproject.toml` in the `tool.poetry.dependencies` section, but `poetry add` will automatically update this file.

### poetry.lock

If this file isn't exist then it will be create after `poetry install` command.

This file contains versions of project dependencies, and `poetry` will use the versions from this file if it exists,
because of that `poetry.lock` should be add to your version control system.

> The addition of a lock file ensures that the versions of dependencies on the developer machine and the user machine are the same.

### venv

You can use external virtual env, but poetry also has built-in virtual env mechanism.

Activate poetry virtual env:

```bash
poetry shell
```

Poetry will create nested shell.
