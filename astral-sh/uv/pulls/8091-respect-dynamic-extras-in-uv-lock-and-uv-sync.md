```yaml
number: 8091
title: "Respect dynamic extras in `uv lock` and `uv sync`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/extras-dynamic
created_at: 2024-10-10T13:43:39Z
updated_at: 2024-10-10T14:00:38Z
url: https://github.com/astral-sh/uv/pull/8091
synced_at: 2026-01-12T16:08:09Z
```

# Respect dynamic extras in `uv lock` and `uv sync`

---

_@charliermarsh_

## Summary

We can't rely on reading these from the `pyproject.toml`; instead, we resolve the project metadata (which will typically just require reading the `pyproject.toml`, but will go through our standard metadata paths).

Closes https://github.com/astral-sh/uv/issues/8071.


---

_Label `bug` added by @charliermarsh on 2024-10-10 13:43_

---

_Merged by @charliermarsh on 2024-10-10 14:00_

---

_Closed by @charliermarsh on 2024-10-10 14:00_

---

_Branch deleted on 2024-10-10 14:00_

---
