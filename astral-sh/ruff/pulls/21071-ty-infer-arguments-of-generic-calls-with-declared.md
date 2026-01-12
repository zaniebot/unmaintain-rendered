```yaml
number: 21071
title: "[ty] Infer arguments of generic calls with declared type context"
type: pull_request
state: closed
author: ibraheemdev
labels:
  - ty
assignees: []
base: main
head: ibraheem/generic-call-argument-tcx
created_at: 2025-10-25T04:52:25Z
updated_at: 2025-11-03T00:32:17Z
url: https://github.com/astral-sh/ruff/pull/21071
synced_at: 2026-01-12T15:57:15Z
```

# [ty] Infer arguments of generic calls with declared type context

---

_@ibraheemdev_

## Summary

Resolves https://github.com/astral-sh/ty/issues/1356.

---

_Review requested from @carljm by @ibraheemdev on 2025-10-25 04:52_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-10-25 04:52_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-10-25 04:52_

---

_Review requested from @dcreager by @ibraheemdev on 2025-10-25 04:52_

---

_Label `ty` added by @ibraheemdev on 2025-10-25 04:52_

---

_@ibraheemdev reviewed on 2025-10-25 04:54_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:513 on 2025-10-25 04:54_

This is really tricky. We infer the list argument to be `list[TD | None]` due to the annotated type of `T` being `TD | None`. I'm not sure there's a clear heuristic to filter away the `None` here (and similarly for any other type than `None`).

---

_Comment by @github-actions[bot] on 2025-10-25 04:54_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-25 04:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
colour (https://github.com/colour-science/colour)
- colour/recovery/otsu2018.py:1597:21: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]) | (bound method dict[Unknown, Unknown].__setitem__(key: Unknown, value: Unknown, /) -> None)` cannot be called with a key of type `Node_Otsu2018` and a value of type `Unknown | list[Unknown] | dict[Unknown, Unknown] | int` on object of type `Unknown | list[Unknown] | dict[Unknown, Unknown] | int`
- colour/recovery/otsu2018.py:1598:21: error[unsupported-operator] Operator `+=` is unsupported between objects of type `list[Unknown]` and `Literal[1]`
- colour/recovery/otsu2018.py:1598:21: error[unsupported-operator] Operator `+=` is unsupported between objects of type `dict[Unknown, Unknown]` and `Literal[1]`
- colour/recovery/otsu2018.py:1601:17: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]) | (bound method dict[Unknown, Unknown].__setitem__(key: Unknown, value: Unknown, /) -> None)` cannot be called with a key of type `Node_Otsu2018` and a value of type `int` on object of type `Unknown | list[Unknown] | dict[Unknown, Unknown] | int`
- colour/recovery/otsu2018.py:1601:54: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | list[Unknown] | dict[Unknown, Unknown] | int`
- colour/recovery/otsu2018.py:1602:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | list[Unknown] | dict[Unknown, Unknown] | int`
- Found 523 diagnostics
+ Found 517 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/components/homekit/config_flow.py:503:20: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `Any | None`
+ homeassistant/components/homekit/config_flow.py:547:20: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `Any | None`
+ homeassistant/components/homekit/config_flow.py:651:20: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `Unknown | None`
+ homeassistant/components/homekit/config_flow.py:654:19: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `Unknown | None`
+ homeassistant/components/homekit/config_flow.py:655:32: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `Unknown | None`
- Found 14119 diagnostics
+ Found 14124 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:5626 on 2025-10-29 19:03_

If I'm following the control flow correctly, this has to happen here because we haven't yet inferred a specialization in the `Bindings`, since that doesn't happen until you call `check_types`. And we need this specialization applied to the parameter type in order to have a type context to use when infering the argument types. Does that sound right?

My worry here is that this specialization that you're building isn't guaranteed to line up with the one that `check_types` will infer — or at least, it won't once we have the better constraint solver, where the inferred specialization can be influenced by constraints coming from e.g. other arguments.

Ideally we'd find a way to be able to reuse the specialization that `Bindings` is already building up for us. Should we consider moving the specialization inference step so that it happens at the end of `match_parameters`, instead of at the beginning of `check_types`? Or as an explicit third step in between the two?

That might be something we can queue up as follow-on work, in order to land this. But I'd like to at least think through what would be involved, since I think might add more blockers to getting the new constraint solver landed.

---

_@dcreager reviewed on 2025-10-29 19:03_

---

_@ibraheemdev reviewed on 2025-10-29 20:30_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5626 on 2025-10-29 20:30_

> If I'm following the control flow correctly, this has to happen here because we haven't yet inferred a specialization in the Bindings, since that doesn't happen until you call check_types. And we need this specialization applied to the parameter type in order to have a type context to use when infering the argument types. Does that sound right?

Yeah that's correct.

> Ideally we'd find a way to be able to reuse the specialization that Bindings is already building up for us. Should we consider moving the specialization inference step so that it happens at the end of match_parameters, instead of at the beginning of check_types? Or as an explicit third step in between the two?

I'm not exactly sure how that would work. Constructing the complete binding specialization is only possible once we have the types of all arguments, which we want to infer using the type context — so there's a bit of a circular dependency here.

Consider:
```py
def f[T](x: list[T], y: T) -> list[T]:
    return x

def lst[T](x: T) -> list[T]:
    return [x]

def _(x: int, y: int | str):
    z: list[int | str] = f(lst(x), y) # error: expected list[int | str], found list[int]
```

In order to infer a specialization for `f`, we must first infer each of its arguments. The call to `lst(x)` has no way of specializing to `int | str` except by using the type-context `list[int | str]` to infer a *more* assignable type, such that it passes `check_types` once the complete specialization is inferred. Note that without the type-context, pyright also fails to type-check, which suggests it's using the type context in a similar way to this PR.

The question then is, can using the extra type-context lead to a *less* assignable type (as I think you are suggesting)? I'm not sure, as the specialized return type must be assignable to annotated type of the call expression. The only issue I see is the question of how to narrow the type context such that we don't overly widen the inferred type of the call, e.g.
```py
def f[T](x: list[T]) -> T:
    return x[0]

def lst[T](x: T) -> list[T]:
    return [x]

def _(x: int, y: int | str):
    z: int | str = f(lst(x))
    reveal_type(z) # should be int, not int | str
```

---

_Comment by @ibraheemdev on 2025-11-03 00:32_

Superceded by https://github.com/astral-sh/ruff/pull/21210.

---

_Closed by @ibraheemdev on 2025-11-03 00:32_

---
