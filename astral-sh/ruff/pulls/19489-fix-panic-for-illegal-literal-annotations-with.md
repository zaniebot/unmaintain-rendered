```yaml
number: 19489
title: "Fix panic for illegal `Literal[…]` annotations with inner subscript expressions"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: david/fix-literal-subscript-panic
created_at: 2025-07-22T14:03:53Z
updated_at: 2025-07-22T14:07:34Z
url: https://github.com/astral-sh/ruff/pull/19489
synced_at: 2026-01-10T17:58:13Z
```

# Fix panic for illegal `Literal[…]` annotations with inner subscript expressions

---

_Pull request opened by @sharkdp on 2025-07-22 14:03_

## Summary

Fixes pull-types panics for illegal annotations like `Literal[object[index]]`.

Originally reported by @AlexWaygood

## Test Plan

* Verified that this caused panics in the playground, when typing (and potentially hovering over) `x: Literal[obj[0]]`.
* Added a regression test

---

_Review requested from @carljm by @sharkdp on 2025-07-22 14:03_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-22 14:03_

---

_Review requested from @dcreager by @sharkdp on 2025-07-22 14:03_

---

_Label `bug` added by @sharkdp on 2025-07-22 14:03_

---

_Label `ty` added by @sharkdp on 2025-07-22 14:03_

---

_Label `server` added by @AlexWaygood on 2025-07-22 14:05_

---

_@AlexWaygood approved on 2025-07-22 14:05_

---

_Merged by @sharkdp on 2025-07-22 14:07_

---

_Closed by @sharkdp on 2025-07-22 14:07_

---

_Branch deleted on 2025-07-22 14:07_

---

_Comment by @github-actions[bot] on 2025-07-22 14:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---
