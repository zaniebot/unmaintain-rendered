---
number: 6952
title: Rules NPY001 cause infinite loop
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-08-28T19:56:06Z
updated_at: 2023-09-06T12:24:39Z
url: https://github.com/astral-sh/ruff/issues/6952
synced_at: 2026-01-07T13:12:15-06:00
---

# Rules NPY001 cause infinite loop

---

_Issue opened by @qarmin on 2023-08-28 19:56_

Ruff 0.0.286 (latest changes from main branch)

```
ruff  *.py --select NPY001 --no-cache
```

file content:
```
"""Simple dam break uing ANUGA."""
from numpy import float, zeros


def height(x, y):
    zeros(len(x), float)
```

error:
```
This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `numerical_dam_break_dry2463 (32nd copy)07406554559.py`, the rule codes ANN001, ANN201, ARG001, CPY001, D103, NPY001, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

numerical_dam_break_dry2463 (32nd copy)07406554559.py:1:1: CPY001 Missing copyright notice at top of file
numerical_dam_break_dry2463 (32nd copy)07406554559.py:5:5: ANN201 Missing return type annotation for public function `height`
numerical_dam_break_dry2463 (32nd copy)07406554559.py:5:5: D103 Missing docstring in public function
numerical_dam_break_dry2463 (32nd copy)07406554559.py:5:12: ANN001 Missing type annotation for function argument `x`
numerical_dam_break_dry2463 (32nd copy)07406554559.py:5:15: ANN001 Missing type annotation for function argument `y`
numerical_dam_break_dry2463 (32nd copy)07406554559.py:5:15: ARG001 Unused function argument: `y`
numerical_dam_break_dry2463 (32nd copy)07406554559.py:6:19: NPY001 Type alias `np.float` is deprecated, replace with builtin type

```



[numerical_dam_break_dry2463 (32nd copy)07406554559.py.zip](https://github.com/astral-sh/ruff/files/12457800/numerical_dam_break_dry2463.32nd.copy.07406554559.py.zip)


---

_Label `bug` added by @zanieb on 2023-08-28 20:05_

---

_Label `fuzzer` added by @zanieb on 2023-08-28 20:05_

---

_Renamed from "Rules ANN001, ANN201, ARG001, CPY001, D103, NPY001 cause infinite loop" to "Rules NPY001 cause infinite loop" by @qarmin on 2023-09-02 21:50_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-06 12:11_

---

_Referenced in [astral-sh/ruff#7187](../../astral-sh/ruff/pulls/7187.md) on 2023-09-06 12:14_

---

_Closed by @charliermarsh on 2023-09-06 12:24_

---
