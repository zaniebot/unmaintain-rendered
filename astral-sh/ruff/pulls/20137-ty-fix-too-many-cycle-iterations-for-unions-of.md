```yaml
number: 20137
title: "[ty] Fix 'too many cycle iterations' for unions of literals"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: david/fix-660
created_at: 2025-08-28T14:32:24Z
updated_at: 2025-08-28T14:46:39Z
url: https://github.com/astral-sh/ruff/pull/20137
synced_at: 2026-01-12T15:56:55Z
```

# [ty] Fix 'too many cycle iterations' for unions of literals

---

_@sharkdp_

## Summary

Decrease the maximum number of literals in a union before we collapse to the supertype. The better fix for this will be https://github.com/astral-sh/ty/issues/957, but it is very tempting to solve this for now by simply decreasing the limit by one, to get below the salsa limit of 200.

closes https://github.com/astral-sh/ty/issues/660

## Test Plan

Added a regression test that would previously lead to a "too many cycle iterations" panic.


---

_Review requested from @carljm by @sharkdp on 2025-08-28 14:32_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-08-28 14:32_

---

_Review requested from @dcreager by @sharkdp on 2025-08-28 14:32_

---

_Label `bug` added by @sharkdp on 2025-08-28 14:32_

---

_Label `ty` added by @sharkdp on 2025-08-28 14:32_

---

_Comment by @github-actions[bot] on 2025-08-28 14:34_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-28 14:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-08-28 14:42_

---

_Merged by @sharkdp on 2025-08-28 14:46_

---

_Closed by @sharkdp on 2025-08-28 14:46_

---

_Branch deleted on 2025-08-28 14:46_

---
