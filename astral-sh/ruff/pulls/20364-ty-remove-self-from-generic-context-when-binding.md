```yaml
number: 20364
title: "[ty] Remove `Self` from generic context when binding `Self`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/remove-self-from-generic-context
created_at: 2025-09-12T13:05:02Z
updated_at: 2025-09-15T15:40:11Z
url: https://github.com/astral-sh/ruff/pull/20364
synced_at: 2026-01-12T15:57:00Z
```

# [ty] Remove `Self` from generic context when binding `Self`

---

_@sharkdp_

## Summary

This mainly removes an internal inconsistency, where we didn't remove the `Self` type variable when eagerly binding `Self` to an instance type. It has no observable effect, apparently.

builds on top of https://github.com/astral-sh/ruff/pull/20328

## Test Plan

None

---

_Label `ty` added by @sharkdp on 2025-09-12 13:05_

---

_Comment by @github-actions[bot] on 2025-09-12 13:07_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-12 13:17_

---

_Comment by @github-actions[bot] on 2025-09-12 13:22_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://david-remove-self-from-gener.ecosystem-663.pages.dev/diff)**


---

_Marked ready for review by @sharkdp on 2025-09-15 11:33_

---

_Review requested from @carljm by @sharkdp on 2025-09-15 11:33_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-15 11:33_

---

_Review requested from @dcreager by @sharkdp on 2025-09-15 11:33_

---

_Comment by @github-actions[bot] on 2025-09-15 11:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@dcreager approved on 2025-09-15 14:47_

Oof, good catch!

---

_Comment by @sharkdp on 2025-09-15 14:54_

> Oof, good catch!

You told me that this is might be a problem :-)

---

_Comment by @dcreager on 2025-09-15 14:57_

> You told me that this is might be a problem

And then promptly forgot to look for it in #20328 :sweat_smile: 

---

_Merged by @sharkdp on 2025-09-15 15:40_

---

_Closed by @sharkdp on 2025-09-15 15:40_

---

_Branch deleted on 2025-09-15 15:40_

---
