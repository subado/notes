---
title: "Venv"
date: 2023-12-02T15:16:22+04:00
draft: true
---

A **Distribution package** is a versioned archive file that contains Python packages, modules.

**Packages** are Python modules which can contain other modules or recursively, other packages.

A **module** is the basic unit of code reusability in Python and can be a _pure module_ or an _extension module_.

A **virtual environment** is an isolated Python environment that allows packages to be installed project-wide rather than system-wide.

## Virtual env

### Creating a new environment

The `venv` module allows us to create 'virtual environments'.

```bash
python3 -m venv ENV_DIR
```

Replace ENV_DIR with --help to see all options.

### Activate a virtual env

Activating actually means that `ENV_DIR/bin` will be appended in front of your `PATH` variable,
because of that, you will be forced to use executables from `ENV_DIR/bin/` instead of system-wide.

```bash
source .venv/bin/activate`
```

While the environment is active, pip installs packages project-wide.

### Deactivating a virtual env

Deactivating actually means that all variables changed by `activate` will have their old values.

```bash
deactivate
```

## Installing packages

**pip** is the reference Python package manager.

```bash
pip install PACKAGE
```

Pip supports **requirements file** in which you can declare all dependencies.
The default name for this file is `requirements.txt`.

```bash
pip install -r requirements.txt
```

You can export a list of currently installed packages by

```bash
pip freeze
```
