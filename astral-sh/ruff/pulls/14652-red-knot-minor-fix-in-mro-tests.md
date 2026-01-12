```yaml
number: 14652
title: "[red-knot] Minor fix in MRO tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-mro-test
created_at: 2024-11-28T08:32:05Z
updated_at: 2024-11-28T09:17:17Z
url: https://github.com/astral-sh/ruff/pull/14652
synced_at: 2026-01-12T15:55:48Z
```

# [red-knot] Minor fix in MRO tests

---

_@sharkdp_

## Summary

It looks like we want to use `returns_bool()` here. `bool()` is equal to `False`, and we infer `Literal[False]` for it. Which means that the test here will fail as soon as we treat the body of this `if` as unreachable.




---

_Label `red-knot` added by @sharkdp on 2024-11-28 08:32_

---

_Review requested from @carljm by @sharkdp on 2024-11-28 08:32_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-28 08:32_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-28 08:32_

---

_@AlexWaygood approved on 2024-11-28 08:33_

---

_Comment by @github-actions[bot] on 2024-11-28 09:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @sharkdp on 2024-11-28 09:17_

---

_Closed by @sharkdp on 2024-11-28 09:17_

---

_Branch deleted on 2024-11-28 09:17_

---
