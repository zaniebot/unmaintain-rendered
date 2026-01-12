```yaml
number: 11344
title: "Should my team's project structure work as a uv workspace?"
type: issue
state: closed
author: omer54463
labels:
  - question
assignees: []
created_at: 2025-02-08T23:20:33Z
updated_at: 2025-02-14T20:54:38Z
url: https://github.com/astral-sh/uv/issues/11344
synced_at: 2026-01-12T16:00:34Z
```

# Should my team's project structure work as a uv workspace?

---

_@omer54463_

### Question

My team uses a certain project structure:

1. The code is divided into python packages (each with its own pyproject.toml) inside a source directory.
2. The (pytest) tests are their own module, just like pytest likes it.

Currently, we have a PowerShell installation script that installs all those packages in editable mode, and that's the way we work.

```yaml
- source:
  - package_1:
    - pyproject.toml
    - package_1:
      - __init__.py
      - py.typed
      - ...
  - package_2:
    - pyproject.toml
    - package_2:
      - __init__.py
      - py.typed
      - ...
- test:
  - __init__.py
  - conftest.py
  - unit:
    - __init__.py
    - test_something.py
    - ...
```
I created an [example](https://github.com/omer54463/example) of the same structure, and added my [attempt](https://github.com/omer54463/example/tree/refactor/uv) at converting it into a uv workspace.

I encountered two problems:

1. I can't install development dependencies. `uv pip install .[dev]` fails with "Failed to parse metadata from built wheel".
2. I am not sure if there's an automatic way I can tell uv to install all my workspace packages.

Any help would be much appreciated. uv is awesome and incredibly fast, and I would love to use it!

### Platform

Windows 11 x86_64

### Version

uv 0.5.29 (ca73c4754 2025-02-05)

---

_Label `question` added by @omer54463 on 2025-02-08 23:20_

---

_Closed by @omer54463 on 2025-02-08 23:21_

---

_Reopened by @omer54463 on 2025-02-08 23:25_

---

_Comment by @konstin on 2025-02-10 12:22_

> I can't install development dependencies. uv pip install .[dev] fails with "Failed to parse metadata from built wheel".

Your root pyproject.toml doesn't belong to a package, so `uv pip install .` falls back to a bogus setuptools build, I opened https://github.com/astral-sh/uv/issues/11382 to show a better error message here.

In your example, are you looking for the dev group instead of the dev extra?

> I am not sure if there's an automatic way I can tell uv to install all my workspace packages.

You can do that with `uv sync --all-packages`.


---

_Comment by @omer54463 on 2025-02-10 22:23_

Thanks @konstin!

I'm pretty sure I want the dev group, since I want development dependencies there. According to the [documentation](https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-tables):

- [project.dependencies](https://docs.astral.sh/uv/concepts/projects/dependencies/#project-dependencies): Published dependencies.
- [project.optional-dependencies](https://docs.astral.sh/uv/concepts/projects/dependencies/#optional-dependencies): Published optional dependencies, or "extras".
- [dependency-groups](https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-groups): **_Local dependencies for development._**
- [tool.uv.sources](https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-sources): Alternative sources for dependencies during development.

Those are global to the workspace, where would you put those instead of that workspace-level pyproject.toml (which cannot be installed)?

Also, about that `sync` thing, I just tried it out, and it doesn't seem like it installed my packages:

```
> uv sync --all-packages
> uv pip list
Package           Version
----------------- -------
colorama          0.4.6
iniconfig         2.0.0
mypy              1.15.0
mypy-extensions   1.0.0
packaging         24.2
pluggy            1.5.0
pytest            8.3.4
ruff              0.9.5
typing-extensions 4.12.2
```

As you can see, no editable (or otherwise) installations `calculator` and `calculator-backend`.
Am I missing something?

---

_Comment by @konstin on 2025-02-11 13:04_

> Those are global to the workspace, where would you put those instead of that workspace-level pyproject.toml (which cannot be installed)?

Placing dependency groups in the root is also what I'd recommend here. Dependency groups are subtly different from the other dependency kinds, as you can have `[dependency-groups]` without a `[project]` table, and you can install groups without installing (or building) any project. The project root `pyproject.toml` in your example looks correct for the structure you're trying to achieve.

> As you can see, no editable (or otherwise) installations calculator and calculator-backend.
> Am I missing something?

That's somewhat unintuitive: The packages aren't synced since they don't declare a `[build-system]`, so uv assumed they can't be build. Adding a build system entry, such as uv's default, will make the packages be installed with `uv sync --all-packages`:

```
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

(You can of course use any build backend, not just hatchling)

---

_Comment by @omer54463 on 2025-02-14 20:08_

This worked for me!
I have one more question - I want to enforce a minimal version of `mypy`.
Is there a way to control the version of a tool installed by `uvx` in the workspace level?
Otherwise, I'd have to resort to installing the tools like my example currently does and use `uv run mypy .` (or have a lame setup script that does `uv tool install mypy==1.14.0` and use `uvx mypy .`).

---

_Comment by @omer54463 on 2025-02-14 20:54_

Actually, nvm. `mypy` and `ruff` won't work well anyway in a monorepo when they aren't running in a `venv` that has those packages installed (I imagine you'd get something along the lines of "what the hell is this package my-local-dependency" from either of them).
Thanks @konstin!

---

_Closed by @omer54463 on 2025-02-14 20:54_

---
