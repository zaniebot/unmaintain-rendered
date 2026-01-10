```yaml
number: 11965
title: TCH010 False positive when one argument is a TypeVar.
type: issue
state: open
author: randolf-scholz
labels:
  - bug
  - type-inference
assignees: []
created_at: 2024-06-21T12:33:50Z
updated_at: 2025-10-28T19:13:28Z
url: https://github.com/astral-sh/ruff/issues/11965
synced_at: 2026-01-10T11:09:54Z
```

# TCH010 False positive when one argument is a TypeVar.

---

_Issue opened by @randolf-scholz on 2024-06-21 12:33_

[Rule TCH010](https://docs.astral.sh/ruff/rules/runtime-string-union/) should not apply when either argument is a TypeVar:

```python
from typing import TypeVar
X =TypeVar("X")
X | "test"  # no RuntimeError, because TypeVar implements __or__
```

This matters for instance for recursive type aliases:

```python
NestedMapping: TypeAlias = Mapping[K, V | "NestedMapping[K,V]"]
```

Here, writing `Mapping[K, "V | NestedMapping[K,V]"]` instead would cause errors at runtime, because the type-alias will only expect one argument:

```python
from typing import TypeVar, Mapping, TypeAlias
K = TypeVar("K")
V = TypeVar("V")
NestedMapping: TypeAlias = Mapping[K, V | "NestedMapping[K,V]"]
NestedMapping2: TypeAlias = Mapping[K, "V | NestedMapping2[K,V]"]
NestedMapping[str, float]  # Mapping[str, Union[float, ForwardRef('NestedMapping[K,V]')]]
NestedMapping2[str, float]  # TypeError: Too many arguments
```

---

_Label `type-inference` added by @AlexWaygood on 2024-06-21 12:37_

---

_Comment by @AlexWaygood on 2024-06-21 12:42_

I feel like the only way we could implement this is by special-casing `TypeVar` and certain other special types from the `typing` module. But your use case is fairly persuasive, so I guess that's what we'll have to do :/

---

_Label `bug` added by @AlexWaygood on 2024-06-21 12:42_

---

_Comment by @randolf-scholz on 2025-10-28 19:13_

This probably can be closed with `wont-fix`, as this resolves itself with PEP 695 style type aliases.

---
