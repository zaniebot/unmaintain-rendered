```yaml
number: 19764
title: "[ty] Fix more false positives related to `Generic` or `Protocol` being subscripted with a `ParamSpec` or `TypeVarTuple`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/tvar-tuple-paramspec-etc
created_at: 2025-08-05T13:43:14Z
updated_at: 2025-08-05T14:45:59Z
url: https://github.com/astral-sh/ruff/pull/19764
synced_at: 2026-01-12T15:56:46Z
```

# [ty] Fix more false positives related to `Generic` or `Protocol` being subscripted with a `ParamSpec` or `TypeVarTuple`

---

_@AlexWaygood_

## Summary

This PR fixes more false positives relating to `Generic` and `Protocol` subscripts. Specifically, we currently complain about this, even though it's the correct syntax for creating a class that's generic over a `TypeVarTuple`:

```py
from typing_extensions import Generic, Unpack, TypeVarTuple

Ts = TypeVarTuple("Ts")

class Foo(Generic[Unpack[Ts]]): ...
```

The PR also adds more tests for edge cases that were untested. For example, while we have a test like this, where a `ParamSpec` is combined with a `TypeVar` in the type parameters used to subscript `Generic`:

```py
from typing_extensions import Generic, TypeVar, ParamSpec

T = TypeVar("T")
P = ParamSpec("P")

class Foo(Generic[T, P]): ...
```

We do not currently have a test like this, where a `ParamSpec` is the sole typevarlike in the type parameters list:

```py
from typing_extensions import Generic, ParamSpec

P = ParamSpec("P")

class Foo(Generic[P]): ...
```

An earlier version of #19669 broke this latter case specifically (not the former case), but it's a good thing to explicitly test even if that PR isn't landed, so I'm adding a test for it now.

## Test Plan

`cargo test -p ty_python_semantic --test mdtest`


---

_Label `ty` added by @AlexWaygood on 2025-08-05 13:43_

---

_Comment by @github-actions[bot] on 2025-08-05 13:45_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-05 13:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/frame.py:241:31: error[invalid-argument-type] `@Todo(Inference of subscript on special form)` is not a valid argument to `Generic`
- static_frame/core/index_hierarchy.py:243:33: error[invalid-argument-type] `@Todo(Inference of subscript on special form)` is not a valid argument to `Generic`
- Found 1773 diagnostics
+ Found 1771 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-08-05 13:49_

I'm surprised the typing conformance test suite doesn't contain any examples of legacy generic classes that are generic over a `TypeVarTuple` that is unpacked using `typing.Unpack`

---

_Marked ready for review by @AlexWaygood on 2025-08-05 13:49_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-05 13:49_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-05 13:49_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-05 13:49_

---

_Comment by @MichaReiser on 2025-08-05 14:04_

> I'm surprised the typing conformance test suite doesn't contain any examples of legacy generic classes that are generic over a TypeVarTuple that is unpacked using typing.Unpack

You could add one ;)

---

_@carljm approved on 2025-08-05 14:44_

---

_Merged by @AlexWaygood on 2025-08-05 14:45_

---

_Closed by @AlexWaygood on 2025-08-05 14:45_

---

_Branch deleted on 2025-08-05 14:45_

---
