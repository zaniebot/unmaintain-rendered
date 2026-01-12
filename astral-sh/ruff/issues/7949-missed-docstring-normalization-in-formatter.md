```yaml
number: 7949
title: Missed docstring normalization in formatter
type: issue
state: closed
author: konstin
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-10-13T15:54:10Z
updated_at: 2023-10-16T07:39:25Z
url: https://github.com/astral-sh/ruff/issues/7949
synced_at: 2026-01-12T15:54:47Z
```

# Missed docstring normalization in formatter

---

_@konstin_

Ours:
```python
def ca():
    """Convert flat item list to the nested list representation of a
       multidimensional C array with shape 's'."""
```
Black:
```python
def ca():
    """Convert flat item list to the nested list representation of a
    multidimensional C array with shape 's'."""
```

---

_Label `bug` added by @konstin on 2023-10-13 15:54_

---

_Label `formatter` added by @konstin on 2023-10-13 15:54_

---

_Closed by @konstin on 2023-10-13 15:54_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-10-16 07:39_

---
