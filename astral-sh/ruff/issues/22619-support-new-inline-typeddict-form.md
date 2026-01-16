```yaml
number: 22619
title: Support new inline TypedDict form
type: issue
state: open
author: kaste
labels: []
assignees: []
created_at: 2026-01-16T12:32:55Z
updated_at: 2026-01-16T12:32:55Z
url: https://github.com/astral-sh/ruff/issues/22619
synced_at: 2026-01-16T12:56:19Z
```

# Support new inline TypedDict form

---

_@kaste_

### Summary

The mypy Inline TypedDict syntax is not supported  https://mypy.readthedocs.io/en/latest/typed_dict.html#inline-typeddict-types

https://play.ruff.rs/9dd9008b-3148-42b6-bba6-b9eff0f5a5e2

```py
from typing import TypeAlias

def test_values() -> {"width": int, "description": str}:
    return {"width": 42, "description": "test"}

Y: TypeAlias = {"a": int, "b": int}
type Z = {"a": int, "b": int}
```
```
Diagnostics (7)
Undefined name `width` (F821) [Ln 3, Col 24]
Undefined name `description` (F821) [Ln 3, Col 38]
Undefined name `a` (F821) [Ln 6, Col 18]
Undefined name `b` (F821) [Ln 6, Col 28]
Cannot use `type` alias statement on Python 3.10 (syntax was added in Python 3.12) (invalid-syntax) [Ln 7, Col 1]
Undefined name `a` (F821) [Ln 7, Col 12]
Undefined name `b` (F821) [Ln 7, Col 22]
```

### Version

v0.14.13

---
