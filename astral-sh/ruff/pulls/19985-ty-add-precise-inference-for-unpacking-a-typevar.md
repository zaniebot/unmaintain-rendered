```yaml
number: 19985
title: "[ty] Add precise inference for unpacking a TypeVar if the TypeVar has an upper bound with a precise tuple spec"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/unpack-tvar
created_at: 2025-08-19T09:38:51Z
updated_at: 2025-08-19T21:30:11Z
url: https://github.com/astral-sh/ruff/pull/19985
synced_at: 2026-01-12T15:56:51Z
```

# [ty] Add precise inference for unpacking a TypeVar if the TypeVar has an upper bound with a precise tuple spec

---

_@AlexWaygood_

## Summary

If the upper bound of a TypeVar can be unpacked, then so can the TypeVar, because no solutions of the TypeVar exist that would not be assignable to the TypeVar's upper bound.

This is similar in spirit to https://github.com/astral-sh/ruff/pull/19981!

## Test Plan

Mdtests


---

_Review requested from @carljm by @AlexWaygood on 2025-08-19 09:38_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-19 09:38_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-19 09:38_

---

_Label `ty` added by @AlexWaygood on 2025-08-19 09:38_

---

_@AlexWaygood reviewed on 2025-08-19 09:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:4879 on 2025-08-19 09:39_

Possibly I should also be adding a branch for `Type::TypeVar` here too...? I couldn't immediately find any tests that would fail without that branch, though

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/functions.md`:493 on 2025-08-19 09:40_

I enjoyed writing this test. So cool to see the number of features that are coming together here 😃

---

_@AlexWaygood reviewed on 2025-08-19 09:40_

---

_Comment by @github-actions[bot] on 2025-08-19 09:41_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-19 09:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:473 on 2025-08-19 20:43_

As written, this is a useless use of a typevar (since it appears only once in the signature), which we might at some point want to emit a diagnostic on? This annotation should just be replaced with a direct `tuple[int, str]` instead.

I think we should either have a TODO comment about that, or (maybe less distracting) change this signature so the function e.g. also returns a `T`.

```suggestion
def f(x: T) -> T:
    a, b = x
    reveal_type(a)  # revealed: int
    reveal_type(b)  # revealed: str
    return x
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:462 on 2025-08-19 20:44_

very nit: I've only been using the camel-case spelling to refer to `TypeVar` the Python entity. In prose like this I've been using "type variable" or "typevar" (lowercase)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:479 on 2025-08-19 20:45_

Same as above, this function probably should be written `def x(team: Team[tuple[int, str]]):` instead, unless there's a second use of `T` somewhere in the signature.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/functions.md`:479 on 2025-08-19 20:46_

Same comment as above on these two functions

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:4879 on 2025-08-19 20:47_

I don't think you should — if anything I think you want a `debug_assert` that it's not possible to iterate a typevar in an inferable position, like we have here for instance member access:

https://github.com/astral-sh/ruff/blob/c82e255ca8570a683fcd0cde1e62bd1086837c12/crates/ty_python_semantic/src/types.rs#L2791-L2799 

---

_@dcreager approved on 2025-08-19 20:48_

Love it

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:4879 on 2025-08-19 20:49_

In general, I think if we encounter a `Type::TypeVar` type in any position where we'd end up trying to iterate or unpack it, it means we have a bug elsewhere. So I think if anything, here and probably various other places, if we wanted to add a branch for `Type::TypeVar` it should just contain an `unreachable!(...)` (or maybe to avoid end-user crashes, a `debug_assert!(false, ...)`) to help us catch such bugs early. CC @dcreager to correct me if I'm wrong.

---

_@carljm approved on 2025-08-19 20:49_

Nice!

---

_@AlexWaygood reviewed on 2025-08-19 21:00_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:4879 on 2025-08-19 21:00_

Great -- thanks both!

---

_@AlexWaygood reviewed on 2025-08-19 21:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:462 on 2025-08-19 21:01_

```suggestion
## Unpacking a TypeVar

We can infer precise heterogeneous types from the result of an unpacking operation applied to a
type variable if the type variable's upper bound is a type with a precise tuple spec:
```

---

_@AlexWaygood reviewed on 2025-08-19 21:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:482 on 2025-08-19 21:01_

```suggestion
def x(team: Team[T]) -> Team[T]:
    age, name = team.employees[0]
    reveal_type(age)  # revealed: int
    reveal_type(name)  # revealed: str
    return team
```

---

_@AlexWaygood reviewed on 2025-08-19 21:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/functions.md`:479 on 2025-08-19 21:02_

```suggestion
def f[T: tuple[int, str]](x: T) -> T:
    a, b = x
    reveal_type(a)  # revealed: int
    reveal_type(b)  # revealed: str
    return x

@dataclass
class Team[T: tuple[int, str]]:
    employees: list[T]

def x[T: tuple[int, str]](team: Team[T]) -> Team[T]:
    age, name = team.employees[0]
    reveal_type(age)  # revealed: int
    reveal_type(name)  # revealed: str
    return team
```

---

_Merged by @AlexWaygood on 2025-08-19 21:11_

---

_Closed by @AlexWaygood on 2025-08-19 21:11_

---

_Branch deleted on 2025-08-19 21:11_

---

_@carljm reviewed on 2025-08-19 21:30_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:4879 on 2025-08-19 21:30_

Oops, hadn't seen Doug's comment yet when I wrote and submitted mine! Glad we agree, anyway 😆 

---
