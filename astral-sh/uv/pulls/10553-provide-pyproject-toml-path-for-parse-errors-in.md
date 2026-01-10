```yaml
number: 10553
title: "Provide `pyproject.toml` path for parse errors in `uv venv`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/err
created_at: 2025-01-13T02:58:54Z
updated_at: 2025-01-13T19:06:33Z
url: https://github.com/astral-sh/uv/pull/10553
synced_at: 2026-01-10T11:44:57Z
```

# Provide `pyproject.toml` path for parse errors in `uv venv`

---

_Pull request opened by @charliermarsh on 2025-01-13 02:58_

## Summary

Closes https://github.com/astral-sh/uv/issues/10522.

## Test Plan

```
❯ cargo run venv
warning: Failed to parse `pyproject.toml` during environment creation:
  TOML parse error at line 1, column 1
    |
  1 | [project]
    | ^^^^^^^^^
  `pyproject.toml` is using the `[project]` table, but the required `project.version` field is neither set nor present in the `project.dynamic` list

Using CPython 3.13.0
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
```


---

_Review requested from @zanieb by @charliermarsh on 2025-01-13 02:58_

---

_Label `error messages` added by @charliermarsh on 2025-01-13 02:59_

---

_@charliermarsh reviewed on 2025-01-13 02:59_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/venv.rs`:170 on 2025-01-13 02:59_

This matches the general style and structure of the comparable error we show in settings discovery:

https://github.com/astral-sh/uv/blob/b38d3fec64c03f299cf203e4a7dd9ababdb3c377/crates/uv-settings/src/lib.rs#L82

---

_@zanieb approved on 2025-01-13 18:06_

---

_@konstin approved on 2025-01-13 18:32_

---

_Merged by @charliermarsh on 2025-01-13 19:06_

---

_Closed by @charliermarsh on 2025-01-13 19:06_

---

_Branch deleted on 2025-01-13 19:06_

---
