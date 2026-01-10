---
number: 9762
title: Python version environment variable
type: issue
state: closed
author: NellyWhads
labels: []
assignees: []
created_at: 2024-12-10T03:57:59Z
updated_at: 2024-12-10T09:26:51Z
url: https://github.com/astral-sh/uv/issues/9762
synced_at: 2026-01-10T01:24:45Z
---

# Python version environment variable

---

_Issue opened by @NellyWhads on 2024-12-10 03:57_

Here lies a feature request for a environment variable to configure the python version used by `uv`.

The [current documented interface](https://docs.astral.sh/uv/concepts/python-versions/#python-version-files) is to use a `.python-version` file.

Almost all configuration of the workspace and project can be handled using `workspace.toml`, or `pyproject.toml` files. Most of these settings are also exposed with [aptly named environment variables](https://docs.astral.sh/uv/configuration/environment/).

In detail, the requested implementation would:
1. Check `.python-version` files as currently implemented
2. Override the value with `UV_PYTHON_VERSION` if the variable is set.

This helps build projects and workspaces which conform to the [third principle of the 12factor app](https://12factor.net/config) using environment variables directly, and allows users to use a single source of config such as a [direnv `.envrc` file](https://direnv.net/) to configure a workspace.

Current workaround:
Explicitly use `direnv` instead of a static tool such as `dotenv` or environment variables in a docker file to dynamically create the `.python-version` file in the appropriate directory.

Naming options:
1. `UV_PYTHON_VERSION` to match with `.python-version` file name
2. `UV_PYTHON` to match with the `--python` argument of `uv` commands which interact with `python` environments

---

_Comment by @zanieb on 2024-12-10 05:19_

You can use `UV_PYTHON`, i.e, `UV_PYTHON=3.12`.

---

_Comment by @NellyWhads on 2024-12-10 09:26_

Was this always here? https://docs.astral.sh/uv/configuration/environment/#uv_python

Nice.

---

_Closed by @NellyWhads on 2024-12-10 09:26_

---
