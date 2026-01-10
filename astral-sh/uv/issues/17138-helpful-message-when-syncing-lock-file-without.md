---
number: 17138
title: "Helpful message when syncing lock file without having index metadata from `pyproject.toml`"
type: issue
state: open
author: ketozhang
labels:
  - enhancement
assignees: []
created_at: 2025-12-15T22:42:33Z
updated_at: 2025-12-16T22:50:05Z
url: https://github.com/astral-sh/uv/issues/17138
synced_at: 2026-01-10T01:26:14Z
---

# Helpful message when syncing lock file without having index metadata from `pyproject.toml`

---

_Issue opened by @ketozhang on 2025-12-15 22:42_

### Summary

When deploying Python software from lock files (i.e., both `uv.lock` and `pylock.toml`) for reproducible installs, we like to treat the lock file as compiled from source such that the where we want to deploy the environment to shouldn't need any of its original source code. I consider the `pyproject.toml` as the source code for the lock file.

One pain point, if I am understanding correctly, is that the index metadata in `[[tool.uv.index]]` is lost in the lock file therefore the environment variables `UV_INDEX_FOOBAR_PASSWORD` are not used during `uv sync` or `uv pip sync` without the associated `pyproject.toml` in the working directory. 

`pyproject.toml` is actually not necessary since users may use `uv auth login` to build up their authenticated index list. However, the user must remember to run this for every private index listed in the lock file.

My feature request is that uv understand the index URLS stored in the lock file and will check for authentication to those indices during install missing `pyproject.toml`. On failure, uv should instruct the users to provide index credentials through `uv auth login`. If `UV_INDEX_INDEXNAME_*` is specified, uv should warn these environment variable does not work unless a `pyproject.toml` exist.

### Example

```sh
$ mkdir mysoftware && cd mysoftware
$ wget https://example.com/myapp/pylock.toml
$ uv venv --python 3.14
$ uv pip sync pylock.toml
Failed to fetch:
  `https://privatepypi.example.com/packages/mysoftwaredependency`

No `pyproject.toml` found. If you are trying to install a lock file, please register the index and credentials (see `uv auth login --help`).

Warning UV_INDEX_PRIVATEPYPI_PASSWORD set but `pyproject.toml` is not found. 
```

---

_Label `enhancement` added by @ketozhang on 2025-12-15 22:42_

---

_Comment by @konstin on 2025-12-16 09:23_

Currently, `uv.lock` is needed for a number of features, such as workspace discovery and the indexes you mentioned. If you're using a `[build-system]` table, the `pyproject.toml` is mandatory to build the project - There's too many things that `pyproject.toml` does to treat `uv.lock` as a compiled artifact that works without the source file. This doesn't mean we shouldn't add more information to `uv.lock`, but `pyproject.toml` should still exist in the deployment.

---

_Comment by @ketozhang on 2025-12-16 22:46_

In my case, there isn't a need for a build system since I am provisioning an app where all my package including the entrypoint program is already uploaded to some pypi server. To me a deploy is really just a list of Python packages (the lock file) and some configuration files (_I guess I can treat pyproject.toml as another config file_).

Could simply propagate all the metadata to `[tool]` https://packaging.python.org/en/latest/specifications/pylock-toml/#tool ? Can we start with `[[tool.uv.index]]` and `[tool.uv.sources]`?



---

_Comment by @ketozhang on 2025-12-16 22:50_

Any opposition to adding something of the form of this warning?

```
Warning UV_INDEX_PRIVATEPYPI_PASSWORD set but `pyproject.toml` is not found. 
```

I can't see how the index env var will be useful without a `[[tool.uv.index]]` somewhere (incl. `pyproject.toml` or `uv.toml`). 

---
