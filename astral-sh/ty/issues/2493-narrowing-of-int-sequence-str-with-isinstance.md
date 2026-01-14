```yaml
number: 2493
title: "Narrowing of `int | Sequence[str]` with `isinstance(Sequence)` is not Sequence[str]"
type: issue
state: open
author: henriquegemignani
labels: []
assignees: []
created_at: 2026-01-14T14:29:25Z
updated_at: 2026-01-14T14:29:25Z
url: https://github.com/astral-sh/ty/issues/2493
synced_at: 2026-01-14T14:41:08Z
```

# Narrowing of `int | Sequence[str]` with `isinstance(Sequence)` is not Sequence[str]

---

_@henriquegemignani_

### Summary

Playground: https://play.ty.dev/6143bcb7-6aa9-405d-ba62-33a4ea7e764d

Code:
```python
from collections.abc import Sequence

type JsonType_RO = int | Sequence[str]


def encode_a(value: JsonType_RO) -> None:
    if isinstance(value, Sequence):
        sequence_value: Sequence[str] = value

def encode_b(value: JsonType_RO) -> None:
    if isinstance(value, Sequence[str]):
        sequence_value: Sequence[str] = value
```
Errors:
```
Object of type `(int & Sequence[object]) | Sequence[str]` is not assignable to `Sequence[str]` (invalid-assignment) [Ln 8, Col 41]
Object of type `JsonType_RO` is not assignable to `Sequence[str]` (invalid-assignment) [Ln 12, Col 41]
```

The main case is `encode_a`. I'd expect `value` to be narrowed to `Sequence[str]`, but that's not the case. As far as I could find, that should be valid typing. I tried to find some related issue, but no luck with search.

`encode_b` is just an attempt to investigate further, but it's interesting how that causes ty to not narrow at all.

### Version

ty 0.0.11 (830cb9cc6 2026-01-09)

---
