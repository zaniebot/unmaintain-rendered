```yaml
number: 1565
title: Enforce URL constraints for non-URL dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/constraint
created_at: 2024-02-17T03:07:37Z
updated_at: 2024-02-17T03:11:29Z
url: https://github.com/astral-sh/uv/pull/1565
synced_at: 2026-01-10T15:33:24Z
```

# Enforce URL constraints for non-URL dependencies

---

_Pull request opened by @charliermarsh on 2024-02-17 03:07_

## Summary

This was just a missing line -- we have `dependencies.remove(&package);` in the ~identical branch above, but it must've been an oversight to omit it here.

Closes https://github.com/astral-sh/uv/issues/1467.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2024-02-17 03:07_

---

_Merged by @charliermarsh on 2024-02-17 03:11_

---

_Closed by @charliermarsh on 2024-02-17 03:11_

---

_Branch deleted on 2024-02-17 03:11_

---
