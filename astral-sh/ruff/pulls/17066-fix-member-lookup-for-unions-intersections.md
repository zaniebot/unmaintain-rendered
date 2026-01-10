```yaml
number: 17066
title: "Fix member lookup for unions & intersections ignoring policy"
type: pull_request
state: merged
author: mishamsk
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-member-lookup-with-policy-for-unions-intersections
created_at: 2025-03-31T00:39:54Z
updated_at: 2025-03-31T16:42:59Z
url: https://github.com/astral-sh/ruff/pull/17066
synced_at: 2026-01-10T19:40:36Z
```

# Fix member lookup for unions & intersections ignoring policy

---

_Pull request opened by @mishamsk on 2025-03-31 00:39_

## Summary

A quick fix for how union/intersection member search ins performed in Knot.

## Test Plan

* Added a dunder method call test for Union, which exhibits the error
* Also added an intersection error, but it is not triggering currently due to `call` logic not being fully implemented for intersections.


---

_Review requested from @carljm by @mishamsk on 2025-03-31 00:39_

---

_Review requested from @AlexWaygood by @mishamsk on 2025-03-31 00:39_

---

_Review requested from @sharkdp by @mishamsk on 2025-03-31 00:39_

---

_Review requested from @dcreager by @mishamsk on 2025-03-31 00:39_

---

_Comment by @github-actions[bot] on 2025-03-31 00:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp approved on 2025-03-31 07:20_

Thank you! I moved the new test to a separate section and rewrote it slightly. The tests above have a more literate style and I felt like the new test broke the flow of the original "story" I wanted to tell there.

---

_Label `red-knot` added by @MichaReiser on 2025-03-31 07:31_

---

_Merged by @sharkdp on 2025-03-31 07:40_

---

_Closed by @sharkdp on 2025-03-31 07:40_

---

_Branch deleted on 2025-03-31 16:42_

---
