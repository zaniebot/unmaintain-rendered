```yaml
number: 13567
title: "[red-knot] Remove `Type::RevealType`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: remove-revealtype-variant
created_at: 2024-09-30T12:44:55Z
updated_at: 2024-10-01T10:10:15Z
url: https://github.com/astral-sh/ruff/pull/13567
synced_at: 2026-01-12T15:55:44Z
```

# [red-knot] Remove `Type::RevealType`

---

_@AlexWaygood_

Instead, store the information about the kind of function it is as a field on instances of `FunctionType`. This is more extensible, as it means we won't have to add new `Type` variants for each of `cast`, `runtime_checkable`, `no_type_check`, `type_check_only`, `dataclasses.dataclass`, `dataclasses.field`, etc. etc. Since these special-cased functions work like normal functions in almost every respect, this should lead to much less branching in the long term. We'll be able to have just one or two `match` statements in `Type::call` and similar, instead of adding unnecessary branches for `DisplayType::Display`, `Type::bool`, etc.

See conversation at https://github.com/astral-sh/ruff/pull/13384/files#discussion_r1766972444, and the following comments

---

_Label `red-knot` added by @AlexWaygood on 2024-09-30 12:44_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-30 12:44_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-30 12:44_

---

_Comment by @github-actions[bot] on 2024-09-30 12:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1079 on 2024-09-30 20:31_

nit: let's find an alternative to `Regular` here, since "regular function" is a term of art with a very different meaning.

Maybe "Ordinary" or "Common"? "Simple"?

---

_@carljm approved on 2024-09-30 20:32_

Looks great, thank you!!

---

_@AlexWaygood reviewed on 2024-09-30 21:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1079 on 2024-09-30 21:36_

> "regular function" is a term of art with a very different meaning.

Hah, I had no idea! `Ordinary` SGTM

---

_Merged by @AlexWaygood on 2024-10-01 10:01_

---

_Closed by @AlexWaygood on 2024-10-01 10:01_

---

_Branch deleted on 2024-10-01 10:01_

---
