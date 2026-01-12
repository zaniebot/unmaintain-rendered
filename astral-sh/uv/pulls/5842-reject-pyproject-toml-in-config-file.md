```yaml
number: 5842
title: "Reject `pyproject.toml` in `--config-file`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/c
created_at: 2024-08-06T23:03:02Z
updated_at: 2024-08-06T23:41:00Z
url: https://github.com/astral-sh/uv/pull/5842
synced_at: 2026-01-12T16:07:03Z
```

# Reject `pyproject.toml` in `--config-file`

---

_@charliermarsh_

This already rejects `pyproject.toml`... but because the schema validation is relaxed (we allow unknown fields, and all fields are optional), a `pyproject.toml` doesn't get properly rejected here.

This PR makes the schema stricter, but in a safe way (by adding the other `tool.uv` fields, like `workspace`, as any).

Closes #5832.

---

_Label `bug` added by @charliermarsh on 2024-08-06 23:04_

---

_Merged by @charliermarsh on 2024-08-06 23:40_

---

_Closed by @charliermarsh on 2024-08-06 23:40_

---

_Branch deleted on 2024-08-06 23:41_

---
