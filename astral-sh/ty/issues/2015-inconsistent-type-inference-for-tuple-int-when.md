```yaml
number: 2015
title: "Inconsistent type inference for `tuple[int, ...]` when using `type` and `TypeAlias`"
type: issue
state: closed
author: tomasr8
labels:
  - bug
  - type aliases
assignees: []
created_at: 2025-12-17T14:44:17Z
updated_at: 2026-01-15T00:18:56Z
url: https://github.com/astral-sh/ty/issues/2015
synced_at: 2026-01-15T00:41:56Z
```

# Inconsistent type inference for `tuple[int, ...]` when using `type` and `TypeAlias`

---

_@tomasr8_

### Summary

Wasn't sure how to describe this exactly but here's a small reproducer. Notice that `foo_slice` is either inferred to be `tuple[int, int]` or `tuple[int, ...]` depending on how `Version` is defined:

#### Using `TypeAlias`

```python
Version: TypeAlias = tuple[int, int, int]

def foo() -> Version:
    return (1, 2, 3)

# Inferred as tuple[int, int]
foo_slice = foo()[:2]
```

#### Using `type` keyword

```python
type Version = tuple[int, int, int]

def foo() -> Version:
    return (1, 2, 3)

# Inferred as tuple[int, ...]
foo_slice = foo()[:2]
```

I'd expect both to behave the same, ruff even recommends to use the latter..

playground link: https://play.ty.dev/d8cdb949-9e7e-48e5-8866-f41eac402931

### Version

ty 0.0.2

---

_Label `bug` added by @AlexWaygood on 2025-12-17 15:16_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-17 15:17_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-12-17 15:25_

---

_Comment by @carljm on 2025-12-17 22:27_

Thanks for the report, definitely a bug.

---

_Label `type aliases` added by @mtshiba on 2026-01-05 14:50_

---

_Closed by @AlexWaygood on 2026-01-15 00:18_

---
