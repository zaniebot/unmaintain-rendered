```yaml
number: 17916
title: "[ty] Ecosystem checks: activate running on 'manticore'"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/fix-17863
created_at: 2025-05-07T13:11:21Z
updated_at: 2025-05-07T13:27:38Z
url: https://github.com/astral-sh/ruff/pull/17916
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Ecosystem checks: activate running on 'manticore'

---

_Pull request opened by @sharkdp on 2025-05-07 13:11_

## Summary

This is sort of an anticlimactic resolution to astral-sh/ty#96, but now that we understand what the root cause for the stack overflows was, I think it's fine to enable running on this project. See the linked ticket for the full analysis.

closes astral-sh/ty#96

## Test Plan

Ran lots of times locally and never observed a crash at worker thread stack sizes > 8 MiB.


---

_Review requested from @carljm by @sharkdp on 2025-05-07 13:11_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-07 13:11_

---

_Review requested from @dcreager by @sharkdp on 2025-05-07 13:11_

---

_Label `testing` added by @sharkdp on 2025-05-07 13:11_

---

_Label `ty` added by @sharkdp on 2025-05-07 13:11_

---

_Comment by @github-actions[bot] on 2025-05-07 13:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @carljm on 2025-05-07 13:27_

---

_Closed by @carljm on 2025-05-07 13:27_

---

_Branch deleted on 2025-05-07 13:27_

---
