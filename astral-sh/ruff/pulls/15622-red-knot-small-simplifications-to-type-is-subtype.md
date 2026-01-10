```yaml
number: 15622
title: "[red-knot] Small simplifications to `Type::is_subtype_of` and `Type::is_disjoint_from`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/type-t-subtype
created_at: 2025-01-20T19:02:08Z
updated_at: 2025-01-22T00:31:57Z
url: https://github.com/astral-sh/ruff/pull/15622
synced_at: 2026-01-10T20:05:43Z
```

# [red-knot] Small simplifications to `Type::is_subtype_of` and `Type::is_disjoint_from`

---

_Pull request opened by @AlexWaygood on 2025-01-20 19:02_

## Summary

This PR generalizes some of the logic we have in `Type::is_subtype_of` and `Type::is_disjoint_from` so that we fallback to the instance type of the metaclass more often in `Type::ClassLiteral` and `Type::SubclassOf` branches. This simplifies the code (we end up with one less branch in `is_subtype_of`, and we can remove a helper method that's no longer used), makes the code more robust (any fixes made to subtyping or disjointness of instance types will automatically improve our understanding of subtyping/disjointness for class-literal types and `type[]` types) and more elegantly expresses the type-system invariants encoded in these branches.

## Test Plan

No new tests added (it's a pure refactor, adding no new functionality). All existing tests pass, however, including the property tests.


---

_Comment by @github-actions[bot] on 2025-01-20 19:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @AlexWaygood on 2025-01-20 19:34_

---

_Renamed from "[red-knot] Simplify and generalize `Type::is_subtype_of` a little bit" to "[red-knot] Small simplifications to `Type::is_subtype_of` and `Type::is_disjoint_from`" by @AlexWaygood on 2025-01-21 13:13_

---

_Marked ready for review by @AlexWaygood on 2025-01-21 13:13_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-21 13:13_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-21 13:13_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-21 13:13_

---

_Label `internal` added by @AlexWaygood on 2025-01-21 13:14_

---

_@carljm approved on 2025-01-22 00:16_

Looks good!

---

_Merged by @AlexWaygood on 2025-01-22 00:31_

---

_Closed by @AlexWaygood on 2025-01-22 00:31_

---

_Branch deleted on 2025-01-22 00:31_

---
