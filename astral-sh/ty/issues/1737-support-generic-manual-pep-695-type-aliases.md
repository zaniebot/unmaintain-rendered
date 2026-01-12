```yaml
number: 1737
title: "Support generic \"manual\" PEP 695 type aliases"
type: issue
state: open
author: sharkdp
labels:
  - typing semantics
  - type aliases
assignees: []
created_at: 2025-12-03T08:19:19Z
updated_at: 2025-12-19T12:12:02Z
url: https://github.com/astral-sh/ty/issues/1737
synced_at: 2026-01-12T15:54:25Z
```

# Support generic "manual" PEP 695 type aliases

---

_@sharkdp_

Support generic PEP 695 aliases if they are manually constructed via `TypeAliasType(…)`:

```py
from typing import TypeAliasType, TypeVar, reveal_type

K = TypeVar("K")
V = TypeVar("V")

MyDict = TypeAliasType("MyDict", dict[K, V], type_params=(K, V))


def _(x: MyDict[str, int]):
    reveal_type(x)  # should be: dict[str, int]
```

---

_Label `typing semantics` added by @sharkdp on 2025-12-03 08:19_

---

_Added to milestone `Stable` by @sharkdp on 2025-12-03 09:56_

---

_Assigned to @sharkdp by @sharkdp on 2025-12-04 20:54_

---

_Label `type aliases` added by @AlexWaygood on 2025-12-19 12:12_

---
