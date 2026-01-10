```yaml
number: 18377
title: "[ty] _typeshed.Self is not a special form"
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: cjm/fix-self
created_at: 2025-05-29T23:36:29Z
updated_at: 2025-05-30T06:35:04Z
url: https://github.com/astral-sh/ruff/pull/18377
synced_at: 2026-01-10T18:45:04Z
```

# [ty] _typeshed.Self is not a special form

---

_Pull request opened by @carljm on 2025-05-29 23:36_

## Summary

This change was based on a mis-reading of a comment in typeshed, and a wrong assumption about what was causing a test failure in a prior PR. Reverting it doesn't cause any tests to fail.

## Test Plan

Existing tests.


---

_Review requested from @AlexWaygood by @carljm on 2025-05-29 23:36_

---

_Label `ty` added by @carljm on 2025-05-29 23:36_

---

_Review requested from @sharkdp by @carljm on 2025-05-29 23:36_

---

_Review requested from @dcreager by @carljm on 2025-05-29 23:36_

---

_Review request for @dcreager removed by @carljm on 2025-05-29 23:36_

---

_Review request for @sharkdp removed by @carljm on 2025-05-29 23:36_

---

_Comment by @github-actions[bot] on 2025-05-29 23:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @carljm on 2025-05-30 00:11_

---

_Closed by @carljm on 2025-05-30 00:11_

---

_Branch deleted on 2025-05-30 00:11_

---

_Label `internal` added by @AlexWaygood on 2025-05-30 06:35_

---
