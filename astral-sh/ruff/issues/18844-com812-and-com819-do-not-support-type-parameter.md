```yaml
number: 18844
title: COM812 and COM819 do not support type parameter lists
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2025-06-21T01:31:17Z
updated_at: 2025-07-28T14:53:05Z
url: https://github.com/astral-sh/ruff/issues/18844
synced_at: 2026-01-12T15:54:56Z
```

# COM812 and COM819 do not support type parameter lists

---

_@dscorbett_

### Summary

[`missing-trailing-comma` (COM812)](https://docs.astral.sh/ruff/rules/missing-trailing-comma/) and [`prohibited-trailing-comma` (COM819)](https://docs.astral.sh/ruff/rules/prohibited-trailing-comma/) have false negatives for type parameter lists, wherein trailing commas make no difference. The comma rules for type parameter lists should be the same as for normal parameter lists.

COM812 should fix this:
```python
type X[
    T
] = T
def f[
    T
](): pass
class C[
    T
]: pass
```
to this:
```python
type X[
    T,
] = T
def f[
    T,
](): pass
class C[
    T,
]: pass
```

COM819 should fix this:
```python
type X[T,] = T
def f[T,](): pass
class C[T,]: pass
```
to this:
```python
type X[T] = T
def f[T](): pass
class C[T]: pass
```

### Version

ruff 0.12.0 (87f0feb21 2025-06-17)

---

_Label `bug` added by @MichaReiser on 2025-06-23 06:52_

---

_Label `rule` added by @MichaReiser on 2025-06-23 06:52_

---

_Label `help wanted` added by @MichaReiser on 2025-06-23 06:52_

---

_Closed by @ntBre on 2025-07-28 14:53_

---
