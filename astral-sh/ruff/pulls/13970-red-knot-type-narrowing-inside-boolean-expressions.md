```yaml
number: 13970
title: "[red-knot] Type narrowing inside boolean expressions "
type: pull_request
state: merged
author: TomerBin
labels:
  - ty
assignees: []
merged: true
base: main
head: tomer/boolean-operations-type-narrowing
created_at: 2024-10-28T21:29:04Z
updated_at: 2024-10-29T08:22:32Z
url: https://github.com/astral-sh/ruff/pull/13970
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Type narrowing inside boolean expressions 

---

_Pull request opened by @TomerBin on 2024-10-28 21:29_

## Summary

This PR adds type narrowing in `and` and `or` expressions, for example:

```py
class A: ...

x: A | None = A() if bool_instance() else None

isinstance(x, A) or reveal_type(x)  # revealed: None
``` 

## Test Plan
New mdtests 😍 

<!-- How was it tested? -->


---

_Renamed from "[red-knot] Narrowing - Boolean expressions " to "[red-knot] Type narrowing inside boolean expressions " by @TomerBin on 2024-10-28 21:31_

---

_Marked ready for review by @TomerBin on 2024-10-28 21:36_

---

_Review requested from @carljm by @TomerBin on 2024-10-28 21:36_

---

_Review requested from @MichaReiser by @TomerBin on 2024-10-28 21:36_

---

_Review requested from @AlexWaygood by @TomerBin on 2024-10-28 21:36_

---

_Comment by @github-actions[bot] on 2024-10-28 21:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @AlexWaygood on 2024-10-28 22:47_

---

_@carljm approved on 2024-10-29 01:17_

So nice!

---

_Merged by @carljm on 2024-10-29 01:17_

---

_Closed by @carljm on 2024-10-29 01:17_

---

_Branch deleted on 2024-10-29 08:22_

---
