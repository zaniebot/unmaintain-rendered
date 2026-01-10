```yaml
number: 279
title: "`numpy` typing support"
type: issue
state: closed
author: my1e5
labels:
  - typing semantics
assignees: []
created_at: 2025-05-08T16:24:35Z
updated_at: 2025-05-16T08:17:20Z
url: https://github.com/astral-sh/ty/issues/279
synced_at: 2026-01-10T02:34:09Z
```

# `numpy` typing support

---

_Issue opened by @my1e5 on 2025-05-08 16:24_

### Summary

Wanted to open this issue to highlight some areas of `numpy` typing that aren't yet covered by ty.

### MRE

```py
# foo.py
import numpy as np
import numpy.typing as npt


x: np.float64 = np.float32(0.0)    # should be x: np.float32

y: npt.NDArray[np.integer] = np.linspace(0.0, 1.0, 10)    # should be y: npt.NDArray[np.floating]

```

### mypy
```
$ uvx --with numpy==2.2.5 mypy --strict foo.py
foo.py:5: error: Incompatible types in assignment (expression has type "floating[_32Bit]", variable has type "float64")  [assignment]
foo.py:7: error: Incompatible types in assignment (expression has type "ndarray[tuple[int, ...], dtype[floating[Any]]]", variable has type "ndarray[tuple[int, ...], dtype[integer[Any]]]")  [assignment]
Found 2 errors in 1 file (checked 1 source file)
```

### ty
```
$ uvx --with numpy==2.2.5 ty check foo.py
All checks passed!
```

### Version

ty 0.0.0-alpha.7 (905a3e1e5 2025-05-07)

---

_Comment by @sharkdp on 2025-05-13 10:29_

Thank you for reporting this. I think this is currently blocked by [missing support for `typing.TypeAlias`](https://github.com/astral-sh/ty/issues/221) (and potentially other things).

---

_Label `typing semantics` added by @sharkdp on 2025-05-13 10:30_

---

_Comment by @sharkdp on 2025-05-16 08:17_

Closing this in favor of https://github.com/astral-sh/ty/issues/221

---

_Closed by @sharkdp on 2025-05-16 08:17_

---
