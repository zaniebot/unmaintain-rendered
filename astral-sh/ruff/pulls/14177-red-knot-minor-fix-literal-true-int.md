```yaml
number: 14177
title: "[red-knot] Minor: fix `Literal[True] <: int`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-bool-literal-subtype-int
created_at: 2024-11-07T22:09:47Z
updated_at: 2024-11-07T22:29:05Z
url: https://github.com/astral-sh/ruff/pull/14177
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Minor: fix `Literal[True] <: int`

---

_@sharkdp_

## Summary

Minor fix to `Type::is_subtype_of` to make sure that Boolean literals are subtypes of `int`, to match runtime semantics.

Found this while doing some [property-testing experiments](https://github.com/astral-sh/ruff/pull/14178).

## Test Plan

New unit test.

---

_Label `red-knot` added by @sharkdp on 2024-11-07 22:09_

---

_Review requested from @carljm by @sharkdp on 2024-11-07 22:09_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-07 22:09_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-07 22:09_

---

_@carljm approved on 2024-11-07 22:11_

Thank you!

---

_Merged by @sharkdp on 2024-11-07 22:23_

---

_Closed by @sharkdp on 2024-11-07 22:23_

---

_Branch deleted on 2024-11-07 22:23_

---

_Comment by @github-actions[bot] on 2024-11-07 22:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
