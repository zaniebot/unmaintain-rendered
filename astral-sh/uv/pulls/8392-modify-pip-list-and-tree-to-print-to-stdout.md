```yaml
number: 8392
title: Modify pip list and tree to print to stdout regardless of the --quiet flag
type: pull_request
state: merged
author: flyaroundme
labels: []
assignees: []
merged: true
base: main
head: ignore-quiet-flag-for-pip-list-and-tree
created_at: 2024-10-20T20:24:56Z
updated_at: 2024-10-20T23:24:34Z
url: https://github.com/astral-sh/uv/pull/8392
synced_at: 2026-01-12T16:08:18Z
```

# Modify pip list and tree to print to stdout regardless of the --quiet flag

---

_@flyaroundme_

## Summary

The desired behavior for `uv tree` and `uv pip list` with `-q | --quiet` flag is https://github.com/astral-sh/uv/issues/8379#issuecomment-2425093709 to still produce output. This is implemented here.

Closes https://github.com/astral-sh/uv/issues/8379

## Test Plan

Use `uv tree -q` or `uv pip list -q` on any uv project setup and expect the corresponding output.
Added tests for that as well.


---

_@charliermarsh approved on 2024-10-20 22:57_

---

_Merged by @charliermarsh on 2024-10-20 23:24_

---

_Closed by @charliermarsh on 2024-10-20 23:24_

---
