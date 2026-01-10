```yaml
number: 11765
title: "[red-knot] support if-expressions in type inference and CFG"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/ifexpr
created_at: 2024-06-05T22:07:58Z
updated_at: 2024-06-06T10:40:45Z
url: https://github.com/astral-sh/ruff/pull/11765
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] support if-expressions in type inference and CFG

---

_Pull request opened by @carljm on 2024-06-05 22:07_

## Summary

Add support for if-expressions. Fixes #11743.

## Test Plan

Added tests.


---

_Review requested from @MichaReiser by @carljm on 2024-06-05 22:07_

---

_Label `red-knot` added by @carljm on 2024-06-05 22:08_

---

_Review request for @MichaReiser removed by @carljm on 2024-06-05 22:08_

---

_Review requested from @AlexWaygood by @carljm on 2024-06-05 22:08_

---

_Review requested from @MichaReiser by @carljm on 2024-06-05 22:08_

---

_Comment by @github-actions[bot] on 2024-06-05 22:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-06-05 22:33_

Could also consider adding a test with a more complex expression, such as

```py
x = 1 if 0 else 2 if 0 else 3 if 0 else 42
```

---

_Comment by @carljm on 2024-06-05 22:41_

I will add a test like that, but I'm going to add it in the next PR, because the only thing it reveals is that we don't yet normalize union types like we should (flattening nested unions), and I want to fix that in the same PR I add the test.

---

_@MichaReiser approved on 2024-06-06 06:07_

---

_Merged by @carljm on 2024-06-06 10:40_

---

_Closed by @carljm on 2024-06-06 10:40_

---

_Branch deleted on 2024-06-06 10:40_

---
