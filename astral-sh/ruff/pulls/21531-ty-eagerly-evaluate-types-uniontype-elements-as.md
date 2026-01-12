```yaml
number: 21531
title: "[ty] Eagerly evaluate `types.UnionType` elements as type expressions"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/eager-in-type-expression
created_at: 2025-11-19T20:58:31Z
updated_at: 2025-11-20T16:28:50Z
url: https://github.com/astral-sh/ruff/pull/21531
synced_at: 2026-01-12T15:57:26Z
```

# [ty] Eagerly evaluate `types.UnionType` elements as type expressions

---

_@sharkdp_

## Summary

Eagerly evaluate the elements of a PEP 604 union in value position (e.g. `IntOrStr = int | str`) as type expressions and store the result (the corresponding `Type::Union` if all elements are valid type expressions, or the first encountered `InvalidTypeExpressionError`) on the `UnionTypeInstance`, such that the `Type::Union(…)` does not need to be recomputed every time the implicit type alias is used in a type annotation.

This might lead to performance improvements for large unions, but is also necessary for correctness, because the elements of the union might refer to type variables that need to be looked up in the scope of the type alias, not at the usage site.

## Test Plan

New Markdown tests

---

_Label `ty` added by @sharkdp on 2025-11-19 20:58_

---

_@sharkdp reviewed on 2025-11-19 21:00_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6748 on 2025-11-19 21:00_

The previous version was also only able to surface the first error. I'm not sure if this is a problem, or something that we can live with.

---

_@sharkdp reviewed on 2025-11-19 21:03_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6745 on 2025-11-19 21:03_

@carljm In the end, this didn't require a new salsa query. Instead, we're now eagerly building that `Type::Union` in case there were no errors, and interning the result. And then we only need to copy the result here when we see a use of this implicit type alias in a type expression.

This means that we do some extra work if someone defines `IntOrStr = int | str` and then never uses it as a type. But that feels okay to me?

---

_@sharkdp reviewed on 2025-11-19 21:05_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/narrow.rs`:217 on 2025-11-19 21:05_

Will look into this tomorrow, but I guess a legacy `Union[int, str]` is not allowed in `isinstance`/`issubclass`. If so, we were definitely not handling it correctly before.

---

_Comment by @astral-sh-bot[bot] on 2025-11-19 21:08_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@AlexWaygood reviewed on 2025-11-19 21:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:217 on 2025-11-19 21:24_

It actually _is_ allowed in isinstance/issubclass, but only on Python 3.10+. And I wouldn't worry too much about older versions than that since they're now end-of-life.

We should probably add some tests for this; it's an edge case but it has come up a couple of times on the mypy issue tracker.

---

_@carljm reviewed on 2025-11-19 22:10_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6745 on 2025-11-19 22:10_

Yeah, that seems reasonable.

---

_@sharkdp reviewed on 2025-11-20 08:06_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6745 on 2025-11-20 08:06_

What's potentially more problematic is the fact that when we have a `LargeUnion = C1 | C2 | … | Cn`, we would build $n - 1$ union types eagerly, instead of just one. But if that turns out to be a problem, we can turn the `union_type` field into a salsa query.

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 12:54_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@AlexWaygood reviewed on 2025-11-20 14:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/narrow/isinstance.md`:297 on 2025-11-20 14:19_

```suggestion
    # TODO: Ideally, this would be an error (requires https://github.com/astral-sh/ty/issues/116)
```

---

_Marked ready for review by @sharkdp on 2025-11-20 14:20_

---

_Review requested from @dcreager by @sharkdp on 2025-11-20 14:20_

---

_@sharkdp reviewed on 2025-11-20 14:20_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:194 on 2025-11-20 14:20_

We still don't support generic implicit type aliases, but this is a step in the right direction. `T@_` is just wrong.

---

_@sharkdp reviewed on 2025-11-20 14:21_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/narrow.rs`:878 on 2025-11-20 14:21_

This was recently added

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/narrow/isinstance.md`:173 on 2025-11-20 14:21_

I guess you could also add a test with `Optional`, for completeness?

```pycon
>>> from typing import *
>>> if isinstance(42, Optional[int]):
...     print('hooray')
...     
hooray
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9406 on 2025-11-20 14:23_

seems like we could add this note to the new struct as well?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/narrow/isinstance.md`:330 on 2025-11-20 14:27_

could we also add an examples like:

```py
ListStrOrInt = Union[list[str], int]

def _(x: dict[int, str] | ListStrOrInt):
    if isinstance(x, ListStrOrInt):
        reveal_type(x)

    if isinstance(x, Union[list[str], int]):
        reveal_type(x)
```

Both fail at runtime, so ideally we wouldn't do any narrowing (or we'd intersect with `Unknown` rather than `list[str] | int`), and ideally we'd flag the error with a diagnostic.

But this very rarely comes up, so it's obviously fine if this fails right now and we just add TODOs to the test

---

_@AlexWaygood approved on 2025-11-20 14:31_

Nice!

---

_Comment by @sharkdp on 2025-11-20 15:12_

Looks like there are minor performance improvements

<img width="1197" height="439" alt="image" src="https://github.com/user-attachments/assets/2783167f-800f-4d27-8b51-8c0b44c40f84" />


---

_Merged by @sharkdp on 2025-11-20 16:28_

---

_Closed by @sharkdp on 2025-11-20 16:28_

---

_Branch deleted on 2025-11-20 16:28_

---
