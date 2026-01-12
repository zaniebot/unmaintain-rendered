```yaml
number: 21325
title: "[ty] Handle annotated `self` parameter in constructor of non-invariant generic classes"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/pep-self
created_at: 2025-11-07T19:17:33Z
updated_at: 2025-11-11T00:46:52Z
url: https://github.com/astral-sh/ruff/pull/21325
synced_at: 2026-01-12T15:57:21Z
```

# [ty] Handle annotated `self` parameter in constructor of non-invariant generic classes

---

_@dcreager_

This manifested as an error when inferring the type of a PEP-695 generic class via its constructor parameters:

```py
class D[T, U]:
    @overload
    def __init__(self: "D[str, U]", u: U) -> None: ...
    @overload
    def __init__(self, t: T, u: U) -> None: ...
    def __init__(self, *args) -> None: ...

# revealed: D[Unknown, str]
# SHOULD BE: D[str, str]
reveal_type(D("string"))
```

This manifested because `D` is inferred to be bivariant in both `T` and `U`. We weren't seeing this in the equivalent example for legacy typevars, since those default to invariant. (This issue also showed up for _covariant_ typevars, so this issue was not limited to bivariance.)

The underlying cause was because of a heuristic that we have in our current constraint solver, which attempts to handle situations like this:

```py
def f[T](t: T | None): ...
f(None)
```

Here, the `None` argument matches the non-typevar union element, so this argument should not add any constraints on what `T` can specialize to. Our previous heuristic would check for this by seeing if the argument type is a subtype of the parameter annotation as a whole — even if it isn't a union! That would cause us to erroneously ignore the `self` parameter in our constructor call, since bivariant classes are equivalent to each other, regardless of their specializations.

The quick fix is to move this heuristic "down a level", so that we only apply it when the parameter annotation is a union. This heuristic should go away completely :crossed_fingers: with the new constraint solver.

---

_Review requested from @carljm by @dcreager on 2025-11-07 19:17_

---

_Review requested from @AlexWaygood by @dcreager on 2025-11-07 19:17_

---

_Label `ty` added by @dcreager on 2025-11-07 19:17_

---

_Review requested from @sharkdp by @dcreager on 2025-11-07 19:17_

---

_Comment by @github-actions[bot] on 2025-11-07 19:20_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-11 00:35:18.990083765 +0000
+++ new-output.txt	2025-11-11 00:35:22.335106572 +0000
@@ -937,7 +937,6 @@
 tuples_type_compat.py:127:13: error[type-assertion-failure] Argument does not have asserted type `@Todo(Support for `typing.TypeAlias`)`
 tuples_type_compat.py:129:13: error[type-assertion-failure] Argument does not have asserted type `tuple[int | str, int]`
 tuples_type_compat.py:130:13: error[type-assertion-failure] Argument does not have asserted type `@Todo(Support for `typing.TypeAlias`)`
-tuples_type_compat.py:153:5: error[type-assertion-failure] Argument does not have asserted type `Sequence[Never]`
 tuples_type_compat.py:157:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[""], Literal[""]]` is not assignable to `tuple[int, str]`
 tuples_type_compat.py:162:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[1], Literal[""]]` is not assignable to `tuple[int, *tuple[str, ...]]`
 tuples_type_compat.py:163:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[""], Literal[1]]` is not assignable to `tuple[int, *tuple[str, ...]]`
@@ -1007,5 +1006,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 1009 diagnostics
+Found 1008 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @github-actions[bot] on 2025-11-07 19:21_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @dcreager on 2025-11-07 20:43_

---

_Comment by @github-actions[bot] on 2025-11-07 20:49_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://dcreager-pep-self.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-pep-self.ecosystem-663.pages.dev/timing))


---

_@AlexWaygood approved on 2025-11-10 21:16_

LGTM

---

_Merged by @dcreager on 2025-11-11 00:46_

---

_Closed by @dcreager on 2025-11-11 00:46_

---

_Branch deleted on 2025-11-11 00:46_

---
