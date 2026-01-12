```yaml
number: 17947
title: "[ty] Support extending `__all__` from an imported module even when the module is not an `ExprName` node"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/more-__all__-2
created_at: 2025-05-08T12:14:18Z
updated_at: 2025-05-08T22:54:21Z
url: https://github.com/astral-sh/ruff/pull/17947
synced_at: 2026-01-12T15:56:08Z
```

# [ty] Support extending `__all__` from an imported module even when the module is not an `ExprName` node

---

_@AlexWaygood_

## Summary

This tweaks our `__all__` resolution logic so that patterns such as `__all__.extend(module.sub.sub.__all__)` are suppported (where the type of `module.sub.sub` is resolved as being a `ModuleLiteral`).

It seems like this doesn't have any impact on any mypy_primer projects, but it simplifies the code, is neutral on the benchmarks, and seems like a pretty reasonable pattern to support.

## Test Plan

added an mdtest that fails on `main`


---

_Label `ty` added by @AlexWaygood on 2025-05-08 12:14_

---

_Comment by @github-actions[bot] on 2025-05-08 12:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Closed by @AlexWaygood on 2025-05-08 12:26_

---

_Reopened by @AlexWaygood on 2025-05-08 12:26_

---

_Marked ready for review by @AlexWaygood on 2025-05-08 12:26_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-08 12:26_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-08 12:26_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-08 12:26_

---

_Renamed from "[ty] Call into type inference more when resolving `__all__` (v2)" to "[ty] Support extending `__all__` from an imported module even when the module is not an `ExprName` node" by @AlexWaygood on 2025-05-08 12:28_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/dunder_all.rs`:112 on 2025-05-08 16:30_

This makes me quite nervous about the increased potential for cycles (and performance regression), if we require inferring types for an entire scope in order to resolve `__all__` in that scope. Would it be possible to instead mark these as standalone expressions in semantic index building, given that the pattern for recognizing these cases is static based on AST shape and doesn't itself require type inference?

---

_@carljm reviewed on 2025-05-08 16:30_

---

_@AlexWaygood reviewed on 2025-05-08 21:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/dunder_all.rs`:112 on 2025-05-08 21:02_

SGTM!

---

_@AlexWaygood reviewed on 2025-05-08 21:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/dunder_all.rs`:112 on 2025-05-08 21:59_

Is https://github.com/astral-sh/ruff/pull/17947/commits/f6cb54969a5a13e242d7c2a9757b33caa196db3a what you were thinking of?

---

_Review requested from @carljm by @AlexWaygood on 2025-05-08 21:59_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:5513 on 2025-05-08 22:47_

Ah yeah, I hadn't thought of this consequence; but this seems like a pretty good way to handle it.

---

_@carljm approved on 2025-05-08 22:48_

Yes, looks good!

---

_Merged by @AlexWaygood on 2025-05-08 22:54_

---

_Closed by @AlexWaygood on 2025-05-08 22:54_

---

_Branch deleted on 2025-05-08 22:54_

---
