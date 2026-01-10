```yaml
number: 20147
title: "[ty] ensure union normalization really normalizes"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/union-norm
created_at: 2025-08-29T02:40:33Z
updated_at: 2025-08-29T16:02:36Z
url: https://github.com/astral-sh/ruff/pull/20147
synced_at: 2026-01-10T17:46:21Z
```

# [ty] ensure union normalization really normalizes

---

_Pull request opened by @carljm on 2025-08-29 02:40_

## Summary

Now that we have `Type::TypeAlias`, which can wrap a union, and the possibility of unions including non-unpacked type aliases (which is necessary to support recursive type aliases), we can no longer assume in `UnionType::normalized_impl` that normalizing each element of an existing union will result in a set of elements that we can order and then place raw into `UnionType` to create a normalized union. It's now possible for those elements to themselves include union types (unpacked from an alias). So instead, we need to feed those elements into the full `UnionBuilder` (with alias-unpacking turned on) to flatten/normalize them, and then order them.

## Test Plan

Added mdtest.


---

_Label `ty` added by @carljm on 2025-08-29 02:40_

---

_Comment by @github-actions[bot] on 2025-08-29 02:42_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-29 02:45_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @carljm on 2025-08-29 02:53_

---

_Review requested from @AlexWaygood by @carljm on 2025-08-29 02:53_

---

_Review requested from @sharkdp by @carljm on 2025-08-29 02:53_

---

_Review requested from @dcreager by @carljm on 2025-08-29 02:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:142 on 2025-08-29 08:41_

nit: Black formats these more concisely if we use `...`, since we pass `--pyi` to blacken-docs

```suggestion
class A: ...
class B: ...
class C: ...
class D: ...
class E: ...
class G[T]: ...
```



---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7678 on 2025-08-29 08:42_

And I don't think we _should_ be using a union here, really (though it's a separate issue) -- I'd wager it's the cause of this panic (though I haven't checked that theory)

https://github.com/astral-sh/ruff/blob/0d7ed32494df58f62ed6b48a3491f258ca7fd086/crates/ty_python_semantic/resources/primer/bad.txt#L16-L17

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7686 on 2025-08-29 08:43_

```suggestion
                        .collect::<Box<_>>(),
```

---

_@AlexWaygood approved on 2025-08-29 08:43_

---

_@carljm reviewed on 2025-08-29 15:31_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7678 on 2025-08-29 15:31_

could be related to constraints -- or it might be fixed by this PR. I'll check

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:131 on 2025-08-29 15:32_

```suggestion
```

---

_@carljm reviewed on 2025-08-29 15:32_

---

_@AlexWaygood reviewed on 2025-08-29 15:33_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7678 on 2025-08-29 15:33_

the pandas-stubs commit that made us start panicking sure did add a TypeVar with a lot of constraints, soo... https://github.com/pandas-dev/pandas-stubs/commit/bf1221eb7ea0e582c30fe233d1f4f5713fce376b

---

_@carljm reviewed on 2025-08-29 16:01_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7678 on 2025-08-29 16:01_

It is fixed by this PR, but I think actually that could be the case even if it was caused by a constrained type var

---

_Merged by @carljm on 2025-08-29 16:02_

---

_Closed by @carljm on 2025-08-29 16:02_

---

_Branch deleted on 2025-08-29 16:02_

---
