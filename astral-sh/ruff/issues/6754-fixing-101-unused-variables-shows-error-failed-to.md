```yaml
number: 6754
title: "Fixing 101 unused variables shows error \"Failed to converge after 100 iterations.\""
type: issue
state: open
author: qarmin
labels:
  - needs-decision
assignees: []
created_at: 2023-08-22T07:19:21Z
updated_at: 2023-08-22T12:52:53Z
url: https://github.com/astral-sh/ruff/issues/6754
synced_at: 2026-01-12T15:54:46Z
```

# Fixing 101 unused variables shows error "Failed to converge after 100 iterations."

---

_@qarmin_

Ruff 0.0.285

```
ruff  *.py --select ALL --no-cache
```

file content:
```
def f():
    x0 = 1
    x1 = 1
    x2 = 1
    x3 = 1
    x4 = 1
    x5 = 1
    x6 = 1
    x7 = 1
    x8 = 1
    x9 = 1
    x10 = 1
    x11 = 1
    x12 = 1
    x13 = 1
    x14 = 1
    x15 = 1
    x16 = 1
    x17 = 1
    x18 = 1
    x19 = 1
    x20 = 1
    x21 = 1
    x22 = 1
    x23 = 1
    x24 = 1
    x25 = 1
    x26 = 1
    x27 = 1
    x28 = 1
    x29 = 1
    x30 = 1
    x31 = 1
    x32 = 1
    x33 = 1
    x34 = 1
    x35 = 1
    x36 = 1
    x37 = 1
    x38 = 1
    x39 = 1
    x40 = 1
    x41 = 1
    x42 = 1
    x43 = 1
    x44 = 1
    x45 = 1
    x46 = 1
    x47 = 1
    x48 =1
    x49 = 1
    x50 = 1
    x51 = 1
    x52 = 1
    x53 = 1
    x54 = 1
    x55 = 1
    x56 = 1
    x57 = 1
    x58 = 1
    x59 = 1
    x60 = 1
    x61 = 1
    x62 = 1
    x63 = 1
    x64 = 1
    x65 = 1
    x66 = 1
    x67 = 1
    x68 = 1
    x69 = 1
    x70 = 1
    x71 = 1
    x72 = 1
    x73 = 1
    x74 = 1
    x75 = 1
    x76 = 1
    x77 = 1
    x78 = 1
    x79 = 1
    x80 = 1
    x81 = 1
    x82 = 1
    x83 = 1
    x84 = 1
    x85 = 1
    x86 = 1
    x87 = 1
    x88 = 1
    x89 = 1
    x90 = 1
    x91 = 1
    x92 = 1
    x93 = 1
    x94 = 1
    x95 = 1
    x96 = 1
    x97 = 1
    x98 = 1
    x99 = 1
    x100 = 1
```

error:
```
error: Failed to converge after 100 iterations.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `PY_FILE_TEST_1909706726.py`, the rule codes ANN201, D100, D103, F841, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```


[PY_FILE_TEST_1909706726.py.zip](https://github.com/astral-sh/ruff/files/12405975/PY_FILE_TEST_1909706726.py.zip)


---

_Comment by @charliermarsh on 2023-08-22 12:52_

This is sort of… working as intended. Though we could make some improvements to the isolation group concept to limit this further.

---

_Label `needs-decision` added by @charliermarsh on 2023-08-22 12:52_

---
