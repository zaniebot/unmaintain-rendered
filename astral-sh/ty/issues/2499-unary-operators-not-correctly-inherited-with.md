```yaml
number: 2499
title: "*Unary* operators not correctly inherited with `NewType(..., float)`"
type: issue
state: closed
author: jpgoldberg
labels:
  - bug
assignees: []
created_at: 2026-01-14T19:43:06Z
updated_at: 2026-01-20T19:05:51Z
url: https://github.com/astral-sh/ty/issues/2499
synced_at: 2026-01-20T20:43:14Z
```

# *Unary* operators not correctly inherited with `NewType(..., float)`

---

_@jpgoldberg_

### Summary

It appears that unary operators were not covered in the fix that resolved #2077, resulting in "unsupported operator" when I uses `-thing_of_type_Foo` where `Foo = NewType("Foo", float)`.

Note that binary operators do *not* trigger this error with ty 0.0.11, but the unary `-` operator does.

## How to reproduce

### Step 1: Create test file

Create a file like `ty_newtype.py` with the content:

```python
from typing import NewType

Foo = NewType("Foo", float)

x = float(0.2)
y = -x  # No issues are reported here.

f = Foo(0.2)
g = 2.0 + f  # No issues reported here (since ty 0.0.10)
h = -f  # ty says "Unary operator `-` is not supported ..."
```

### Step 2: Run `ty check` on iit

```console
# ty check ty_newtype.py
% ty check ty_newtype.py
error[unsupported-operator]: Unary operator `-` is not supported for object of type `Foo`
  --> ty_newtype.py:10:5
   |
 8 | f = Foo(0.2)
 9 | g = 2.0 + f  # No issues reported here (since ty 0.0.10)
10 | h = -f  # ty says "Unary operator `-` is not supported ..."
   |     ^^
   |
info: rule `unsupported-operator` is enabled by default

Found 1 diagnostic

```

## Expected

"Foo = NewType("Foo", float)"

## System info

- ty: ty 0.0.11 (830cb9cc6 2026-01-09)
- OS: System Version: macOS 26.1 (25B78)
- HW: Apple M2 Pro 
- Python: Python 3.14.2



### Version

ty 0.0.11 (830cb9cc6 2026-01-09)

---

_Assigned to @oconnor663 by @MichaReiser on 2026-01-14 19:52_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-14 23:20_

---

_Label `bug` added by @carljm on 2026-01-14 23:20_

---

_Closed by @oconnor663 on 2026-01-20 19:05_

---
