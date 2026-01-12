```yaml
number: 1739
title: Support construction of objects based on generic implicit/PEP-613 type aliases
type: issue
state: open
author: sharkdp
labels:
  - generics
assignees: []
created_at: 2025-12-03T09:37:21Z
updated_at: 2025-12-10T16:58:54Z
url: https://github.com/astral-sh/ty/issues/1739
synced_at: 2026-01-12T15:54:25Z
```

# Support construction of objects based on generic implicit/PEP-613 type aliases

---

_@sharkdp_

We should support construction of objects based on (some) generic implicit/PEP-613 type aliases:

```py
from typing import TypeVar

T = TypeVar("T")
MyList = list[T]

xs = MyList(range(10))  # no error here
reveal_type(xs)  # ideally `list[int]` or `list[int | Unknown]`
```
https://play.ty.dev/ec67de22-349a-4861-b075-d48a52e6bae4

---

_Label `generics` added by @sharkdp on 2025-12-03 09:37_

---

_Added to milestone `Stable` by @sharkdp on 2025-12-03 09:56_

---

_Comment by @tmke8 on 2025-12-10 10:45_

This is for example needed to understand numpy's type annotations, like this one: https://github.com/numpy/numpy/blob/0a5840644cfb0a1bbd4ce1dceddb8dc594e805e6/numpy/_typing/_dtype_like.py#L58 which is used in `np.array`:

```python
import numpy as np
a = np.array([[1, 2], [3, 4]], dtype=np.float64)
reveal_type(a)  # currently ty gives `ndarray[tuple[Any, ...], dtype[Unknown]]`
```
where currently the `dtype` cannot be correctly inferred by ty.

---

_Comment by @sharkdp on 2025-12-10 11:38_

> This is for example needed to understand numpy's type annotations, like this one: [numpy/numpy@`0a58406`/numpy/_typing/_dtype_like.py#L58](https://github.com/numpy/numpy/blob/0a5840644cfb0a1bbd4ce1dceddb8dc594e805e6/numpy/_typing/_dtype_like.py#L58) which is used in `np.array`:

The word "instantiation" was probably not picked wisely here. This ticket concerns explicit *construction* of an object based on a generic type alias, i.e. something like `_SupportsDType(some_compatible_argument)`. Explicit *specialization* of a generic type alias, i.e. something like `_SupportsDType[np.dtype[_ScalarT]]` should already be supported.

The `Unknown` that you're observing here is probably related to #1714 instead(?).

---

_Renamed from "Support instantiation of generic implicit/PEP-613 type aliases" to "Support construction of objects based on generic implicit/PEP-613 type aliases" by @sharkdp on 2025-12-10 11:38_

---
