```yaml
number: 2541
title: sorted() loses Literal type information
type: issue
state: closed
author: gaborbernat
labels: []
assignees: []
created_at: 2026-01-17T05:39:35Z
updated_at: 2026-01-17T06:30:58Z
url: https://github.com/astral-sh/ty/issues/2541
synced_at: 2026-01-17T07:05:09Z
```

# sorted() loses Literal type information

---

_@gaborbernat_

### Summary

`sorted()` on a `list[Literal[...]]` returns `list[str]` instead of preserving the Literal type.

## Minimal reproduction

```python
from typing import Literal

PackageType = Literal["Python", "OCI", "JVM"]

def test() -> None:
    items: list[PackageType] = ["Python", "JVM"]
    sorted_items = sorted(items)
    x: PackageType = sorted_items[0]
```

## Error

```
error[invalid-assignment]: Object of type `str` is not assignable to `Literal["Python", "OCI", "JVM"]`
 --> issue.py:8:8
  |
6 |     items: list[PackageType] = ["Python", "JVM"]
7 |     sorted_items = sorted(items)
8 |     x: PackageType = sorted_items[0]
  |        -----------   ^^^^^^^^^^^^^^^ Incompatible value of type `str`
  |        |
  |        Declared type
```

## Expected behavior

`sorted(list[Literal["Python", "OCI", "JVM"]])` should return `list[Literal["Python", "OCI", "JVM"]]`, not
`list[str]`.

### Version

0.0.12

---

_Comment by @ibraheemdev on 2026-01-17 06:27_

This looks like another instance of https://github.com/astral-sh/ty/issues/1815. We should be considering the variance of `PackageType` in the `list`, not the `Iterable` parameter of `sorted`.

---

_Closed by @ibraheemdev on 2026-01-17 06:27_

---
