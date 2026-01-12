```yaml
number: 19667
title: "[ty] Improve the `Display` for generic `type[]` types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/generic-subclassof-display
created_at: 2025-07-31T18:22:58Z
updated_at: 2025-08-01T16:18:40Z
url: https://github.com/astral-sh/ruff/pull/19667
synced_at: 2026-01-12T15:56:44Z
```

# [ty] Improve the `Display` for generic `type[]` types

---

_@AlexWaygood_

## Summary

Currently for these types, we throw away their specializations when printing their `Display`s, e.g.

```py
from typing import Generic, TypeVar

class Foo[T]: ...

S = TypeVar("S")

class Bar(Generic[S]): ...

def _(x: Foo[int], y: Bar[str], z: list[bytes]):
    reveal_type(type(x))  # revealed: type[Foo]
    reveal_type(type(y))  # revealed: type[Bar]
    reveal_type(type(z))  # revealed: type[list]
```

This PR updates them so that we retain the specialization when printing the `Display`, if it is a specialized generic class under the hood.

## Test Plan

Mdtest added


---

_Review requested from @carljm by @AlexWaygood on 2025-07-31 18:22_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-31 18:22_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-31 18:22_

---

_Label `ty` added by @AlexWaygood on 2025-07-31 18:22_

---

_Label `diagnostics` added by @AlexWaygood on 2025-07-31 18:23_

---

_Comment by @github-actions[bot] on 2025-07-31 18:24_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-31 18:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
- static_frame/test/property/strategies.py:927:17: error[invalid-argument-type] Argument to function `get_index_hierarchy` is incorrect: Expected `(...) -> IndexHierarchy`, found `(bound method type[Index].from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> I) | (bound method <class 'Index'>.from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> I)`
+ static_frame/test/property/strategies.py:927:17: error[invalid-argument-type] Argument to function `get_index_hierarchy` is incorrect: Expected `(...) -> IndexHierarchy`, found `(bound method type[Index[Any]].from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> I) | (bound method <class 'Index'>.from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> I)`

```
</details>
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-07-31 18:44_

I think this one's fairly uncontroversial!

---

_Merged by @AlexWaygood on 2025-07-31 18:45_

---

_Closed by @AlexWaygood on 2025-07-31 18:45_

---

_Branch deleted on 2025-07-31 18:45_

---

_@dcreager reviewed on 2025-08-01 16:18_

:+1: love it

---
