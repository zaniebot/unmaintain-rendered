```yaml
number: 1990
title: "Support recursive extras in direct `pyproject.toml` files"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/rec
created_at: 2024-02-26T20:38:25Z
updated_at: 2024-02-26T20:44:26Z
url: https://github.com/astral-sh/uv/pull/1990
synced_at: 2026-01-12T16:04:49Z
```

# Support recursive extras in direct `pyproject.toml` files

---

_@charliermarsh_

## Summary

When a `pyproject.toml` is provided directly to `uv pip compile`, we were failing to resolve recursive extras. The solution I settled on here is to flatten them recursively when determining the requirements upfront.

Closes https://github.com/astral-sh/uv/issues/1987.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2024-02-26 20:38_

---

_Merged by @charliermarsh on 2024-02-26 20:44_

---

_Closed by @charliermarsh on 2024-02-26 20:44_

---

_Branch deleted on 2024-02-26 20:44_

---
