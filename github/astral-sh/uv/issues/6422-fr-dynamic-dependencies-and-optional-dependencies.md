---
number: 6422
title: "FR: Dynamic dependencies and optional dependencies"
type: issue
state: open
author: chrisrodrigue
labels: []
assignees: []
created_at: 2024-08-22T11:12:41Z
updated_at: 2024-10-11T11:14:10Z
url: https://github.com/astral-sh/uv/issues/6422
synced_at: 2026-01-07T13:12:17-06:00
---

# FR: Dynamic dependencies and optional dependencies

---

_Issue opened by @chrisrodrigue on 2024-08-22 11:12_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Greetings `uv` team and congratulations on the release of `0.3`!

I would like to present a rather vexing issue when it comes to project management.

As you may know, Anaconda has a significant presence in the realm of data science and scientific python usage. Currently, `conda` lags behind current PEP standards and does not support reading dependencies from `pyproject.toml`. It is typically used by orgs that deploy to airgapped environments since it comes bundled with Anaconda alongside a number of other popular packages such as `pytorch`.

These orgs vastly prefer to use modern packaging tools like `uv` in non-airgapped development environments, but this requires carefully maintaining project dependencies for compatibility with Anaconda/`conda` and avoidance of adding new runtime dependencies to the airgapped deployment environments.

One such solution to avoid duplication of dependencies between conda `environment.yml` files and those defined `pyproject.toml` is by utilizing `requirements.txt` in conjunction with dynamic `project.dependencies` (as seen in [Drphoton’s answer here](https://stackoverflow.com/questions/76722680/what-is-the-best-way-to-combine-conda-with-standard-python-packaging-tools-e-g)). 

This allows users to have a project layout like so:

```
Project/
|-- src/
|   |-- __init__.py
|   |-- main.py
|
|-- pyproject.toml
|-- requirements.txt
|-- requirements-dev.txt
```

and a `pyproject.toml` with dynamic dependency fields: 

```toml
[project]
name = "myproject"
# ...
dynamic = ["dependencies", "optional-dependencies"]

[tool.setuptools.dynamic]
dependencies = { file = ["requirements.txt"] }

[tool.setuptools.dynamic.optional-dependencies]
dev = { file = ["requirements-dev.txt"] }
```

This setup allows users to run `conda` in airgapped environments like so:

```
conda create -n ENVNAME "python>=3.12" --file requirements.txt
```

while also letting them use `uv pip` in non-airgapped environments:

`uv pip install -e .`

This approach places a dependency on `setuptools` to read the dynamic dependencies, and is admittedly just a stop-gap solution to make up for conda’s lack of `pyproject.toml` support. 

It would be nice if `uv` supported reading dynamic dependencies such that commands like `uv sync` would work if dependencies were defined in other files (such as split being across numerous `requirements.txt`).

(Eagerly waiting to see if Astral ever offers a product similar to Anaconda that provides a number of high quality MIT-licensed python packages/python interpreter/uv/ruff/etc).

---

_Referenced in [astral-sh/uv#6007](../../astral-sh/uv/issues/6007.md) on 2024-08-22 11:19_

---

_Renamed from "Feat: Dynamic dependencies and optional dependencies" to "FR: Dynamic dependencies and optional dependencies" by @chrisrodrigue on 2024-08-22 11:27_

---

_Comment by @adamtheturtle on 2024-10-11 11:14_

At least better for me than the current situation is having a warning / error when trying to use dynamic dependencies.

---

_Referenced in [astral-sh/uv#8949](../../astral-sh/uv/issues/8949.md) on 2024-12-07 14:26_

---

_Referenced in [edgarrmondragon/hatch-pinned-extra#31](../../edgarrmondragon/hatch-pinned-extra/issues/31.md) on 2025-10-15 07:32_

---
