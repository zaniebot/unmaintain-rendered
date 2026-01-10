```yaml
number: 13781
title: "Unify `zip` and `async_zip` compression methods"
type: pull_request
state: merged
author: corentin-ant
labels:
  - enhancement
assignees: []
merged: true
base: main
head: main
created_at: 2025-06-02T10:47:58Z
updated_at: 2025-06-02T12:39:32Z
url: https://github.com/astral-sh/uv/pull/13781
synced_at: 2026-01-10T11:10:42Z
```

# Unify `zip` and `async_zip` compression methods

---

_Pull request opened by @corentin-ant on 2025-06-02 10:47_

## Summary

#13285 added additional compression methods for `async_zip`, but they should also be added to `zip` to support local wheel installation, on top of the ones retrieved over network.

## Test Plan

Installation of local wheels with alternative compression schemes now works (e.g. `uv add test.whl`)

---

_@konstin approved on 2025-06-02 11:34_

---

_Comment by @konstin on 2025-06-02 11:35_

Can you update the lockfile?

---

_Label `enhancement` added by @konstin on 2025-06-02 11:35_

---

_@konstin approved on 2025-06-02 12:39_

---

_Merged by @konstin on 2025-06-02 12:39_

---

_Closed by @konstin on 2025-06-02 12:39_

---
