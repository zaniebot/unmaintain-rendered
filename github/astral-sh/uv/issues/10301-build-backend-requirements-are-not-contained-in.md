---
number: 10301
title: "Build-backend requirements are not contained in `uv.lock`"
type: issue
state: closed
author: tobiasdiez
labels: []
assignees: []
created_at: 2025-01-05T10:47:01Z
updated_at: 2025-01-05T20:02:27Z
url: https://github.com/astral-sh/uv/issues/10301
synced_at: 2026-01-07T13:12:18-06:00
---

# Build-backend requirements are not contained in `uv.lock`

---

_Issue opened by @tobiasdiez on 2025-01-05 10:47_

Suppose you declare a build system with external dependencies in your  `pyproject.toml`:
https://github.com/astral-sh/uv/blob/a056cb292a501ade27ed194a3cc5bbcb2021126d/scripts/benchmark/pyproject.toml#L20-L22

Then the generated `uv.lock` file doesn't contain this build requirement (e.g. hatchling in this example):
https://github.com/astral-sh/uv/blob/a056cb292a501ade27ed194a3cc5bbcb2021126d/scripts/benchmark/uv.lock#L26-L38

This means it is not possible to generate a consistent build environment using uv.

---

_Comment by @charliermarsh on 2025-01-05 20:02_

You can provide your own build constraints (look for `--build-constraints` in the [CLI reference](https://docs.astral.sh/uv/reference/cli/#uv-pip-compile)), but this is a duplicate of several other issues, e.g., #7052.

---

_Closed by @charliermarsh on 2025-01-05 20:02_

---
