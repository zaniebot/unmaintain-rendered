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
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Infer arguments of generic calls with declared type context

---

_Pull request opened by @ibraheemdev on 2025-10-25 04:52_

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

_Comment by @ibraheemdev on 2025-11-03 00:32_

Superceded by https://github.com/astral-sh/ruff/pull/21210.

---

_Closed by @ibraheemdev on 2025-11-03 00:32_

---
