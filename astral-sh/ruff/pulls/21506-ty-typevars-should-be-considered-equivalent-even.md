```yaml
number: 21506
title: "[ty] Typevars should be considered equivalent even after lazy defaults/bounds/etc are forced"
type: pull_request
state: open
author: dcreager
labels:
  - internal
  - ty
assignees: []
base: main
head: dcreager/typevar-bug
created_at: 2025-11-18T02:59:43Z
updated_at: 2025-11-18T18:37:26Z
url: https://github.com/astral-sh/ruff/pull/21506
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Typevars should be considered equivalent even after lazy defaults/bounds/etc are forced

---

_Pull request opened by @dcreager on 2025-11-18 02:59_

This was a lingering place where we were still using `TypeVarInstance` equality to check whether two typevars are the same. We need to fall back on comparing the typevar `identity`, since that remains the same even after evaluating lazy defaults, bounds, or constraints.

This was showing up as a bug on #20933, since the new constraint solver normalizes types as part of constructing constraint sets. That had the effect of making a `TypeVar` instance fail the argument/parameter assignability check after we apply the inferred specialization in something like

```py
def f[T = int]():
    reveal_type(T)
```

since the argument type has a lazy default, but the normalized/specialized parameter type ends up with the evaluated/eager default.

---

_Label `internal` added by @dcreager on 2025-11-18 02:59_

---

_Review requested from @carljm by @dcreager on 2025-11-18 02:59_

---

_Label `ty` added by @dcreager on 2025-11-18 02:59_

---

_Review requested from @AlexWaygood by @dcreager on 2025-11-18 02:59_

---

_Review requested from @sharkdp by @dcreager on 2025-11-18 02:59_

---

_Comment by @astral-sh-bot[bot] on 2025-11-18 03:01_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-18 03:03_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_@AlexWaygood reviewed on 2025-11-18 14:17_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8160 on 2025-11-18 14:17_

IIUC, all of these `KnownInstance` type variants essentially represent the type of a parameterized special form in a value position. so for `T = TypeVar("T")`, we represent the type of `T` as being a `Type::KnownInstanceType(KnownInstanceType::TypeVar(..))`. Given this, why is it important to check two typevars for equivalence using `.is_same_typevar_as()`, but _not_ do the same for other special forms in value positions (which might be parameterized with typevars). E.g. when comparing whether `T | int` is equivalent to `T | int`, where `T` is a type variable -- here `T | int` will be represented in our model as a `Type::KnownInstanceType(KnownInstanceType::UnionType(..))`, where one of the elements in that union is a `Type::KnownInstanceType(KnownInstanceType::TypeVar(..))`: ISTM we need to account for the possibility of `Type::KnownInstanceType(KnownInstanceType::TypeVar(..))`s being in nested positions here, which will require recursing into the types rather than just using `==` checks

---

_@dcreager reviewed on 2025-11-18 18:37_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:8160 on 2025-11-18 18:37_

Hmm yes I think you're right!

---
