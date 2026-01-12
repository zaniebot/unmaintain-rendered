```yaml
number: 7818
title: "`uv version` enhancement for changing the project version"
type: issue
state: closed
author: butterlyn
labels:
  - duplicate
assignees: []
created_at: 2024-09-30T19:34:08Z
updated_at: 2024-09-30T19:40:21Z
url: https://github.com/astral-sh/uv/issues/7818
synced_at: 2026-01-12T15:59:17Z
```

# `uv version` enhancement for changing the project version

---

_@butterlyn_

Request for an enhancement to the feature `uv version` to enable the command to dynamically set/update the project version (in the `pyproject.toml` file), similar to the Poetry command `poetry version` ([link](https://python-poetry.org/docs/cli/#version)).

**Example proposed functionality**:
`uv version 1.4.2` would update the `pyproject.toml` file with version `1.4.2`.

**Use case**:
To use `uv build` within CI pipelines that include automatic version incrementing, there needs to be a way to dynamically set/update the project version within the CI pipeline. This is currently blocking migration from Poetry to uv for my projects that have CI pipelines with automatic version incrementing.

---

_Comment by @zanieb on 2024-09-30 19:40_

This is a duplicate of #6298 

---

_Closed by @zanieb on 2024-09-30 19:40_

---

_Label `duplicate` added by @zanieb on 2024-09-30 19:40_

---
