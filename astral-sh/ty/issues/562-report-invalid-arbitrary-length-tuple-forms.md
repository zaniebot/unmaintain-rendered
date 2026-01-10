```yaml
number: 562
title: Report invalid arbitrary-length tuple forms
type: issue
state: open
author: MatthewMckee4
labels:
  - typing semantics
assignees: []
created_at: 2025-06-01T12:31:23Z
updated_at: 2025-11-14T14:36:44Z
url: https://github.com/astral-sh/ty/issues/562
synced_at: 2026-01-10T02:06:24Z
```

# Report invalid arbitrary-length tuple forms

---

_Issue opened by @MatthewMckee4 on 2025-06-01 12:31_

From the typing spec: https://typing.python.org/en/latest/spec/tuples.html#tuple-type-form, we currently do not support emitting errors for these examples:
```py
t1: tuple[int, ...]  # OK
t2: tuple[int, int, ...]  # Invalid
t3: tuple[...]  # Invalid
t4: tuple[..., int]  # Invalid
t5: tuple[int, ..., int]  # Invalid
t6: tuple[*tuple[str], ...]  # Invalid
t7: tuple[*tuple[str, ...], ...]  # Invalid
```

---

_Label `typing semantics` added by @AlexWaygood on 2025-06-01 15:40_

---

_Added to milestone `Stable` by @carljm on 2025-11-14 14:36_

---
