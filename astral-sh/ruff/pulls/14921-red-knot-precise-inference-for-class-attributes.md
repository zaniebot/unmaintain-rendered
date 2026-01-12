```yaml
number: 14921
title: "[red-knot] Precise inference for `__class__` attributes on objects of all types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/dunder-class-everywhere
created_at: 2024-12-11T16:50:55Z
updated_at: 2024-12-11T17:32:15Z
url: https://github.com/astral-sh/ruff/pull/14921
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Precise inference for `__class__` attributes on objects of all types

---

_@AlexWaygood_

## Summary

We have a special case for `__class__` carved out in `Class::class_member`. But all objects in Python have a `__class__` attribute, not just classes. This PR extends the special case to objects of all types, not just classes. Not only is this more precise, it also allows us to more easily introspect our own type inference in our mdtests.

## Test Plan

New mdtests added that fail on `main`


---

_Label `red-knot` added by @AlexWaygood on 2024-12-11 16:50_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-11 16:50_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-11 16:50_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-11 16:50_

---

_Comment by @github-actions[bot] on 2024-12-11 16:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-12-11 17:04_

Nice!!

---

_@AlexWaygood reviewed on 2024-12-11 17:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:144 on 2024-12-11 17:22_

(on `main` we infer `Literal[type]` here, which is incorrect: `c` could be a subclass of `str` with a custom metaclass. The most we can infer is that `c.__class__` is some (non-strict) subclass of `type`)

---

_@AlexWaygood reviewed on 2024-12-11 17:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:144 on 2024-12-11 17:22_

actually I'll add this as a comment

---

_Merged by @AlexWaygood on 2024-12-11 17:30_

---

_Closed by @AlexWaygood on 2024-12-11 17:30_

---

_Branch deleted on 2024-12-11 17:30_

---
