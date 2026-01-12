```yaml
number: 5552
title: "`uv venv` should support respecting `pyproject.toml`"
type: issue
state: closed
author: Vigilans
labels: []
assignees: []
created_at: 2024-07-29T06:26:48Z
updated_at: 2024-07-31T13:54:17Z
url: https://github.com/astral-sh/uv/issues/5552
synced_at: 2026-01-12T15:58:56Z
```

# `uv venv` should support respecting `pyproject.toml`

---

_@Vigilans_

As per comment in https://github.com/astral-sh/uv/issues/5258#issuecomment-2241724467, `uv venv` may respect the `requires-python` config specified in `pyproject.toml`. This is a very useful feature to sync a python project not only with its dependencies, but also its python interpreter version.

Currently, seems that only `uv sync` can do this, but `uv sync` has a few shortcomings that makes it inconvenient to use. For example:
* `uv sync` does not have `--seed` option: Synced venv does not have `pip` installed, and even if we manually install pip, the next time `uv sync` is executed the `pip` gets removed.
* `uv sync` cannot have custom venv prompt set.
* Custom user demands as specified in #5258

So, I think the feature described in this issue would be the entrypoint for user to tweak their environment with all dependencies (including python itself) specified in `pyproject.toml`, instead of having to pass the python version through command line or environment variables.

---

_Closed by @zanieb on 2024-07-31 13:54_

---
