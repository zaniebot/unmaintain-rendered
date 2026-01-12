```yaml
number: 12416
title: "NPY201: replacing `sometrue`/`alltrue` with `any`/`all` results in broken code"
type: issue
state: closed
author: radoering
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-07-20T05:43:29Z
updated_at: 2024-07-23T09:21:12Z
url: https://github.com/astral-sh/ruff/issues/12416
synced_at: 2026-01-12T15:54:51Z
```

# NPY201: replacing `sometrue`/`alltrue` with `any`/`all` results in broken code

---

_@radoering_

NPY201 can produce broken code. Replacing `numpy.sometrue` with `any` is dangerous and does not work for multidimensional arrays. Using `numpy.any` instead is probably a better option.

numpy 1.26.4:

```
>>> import numpy as np
>>> a = np.array([[False, False], [False, False]])
>>> np.sometrue(a)
False
>>> np.any(a)
False
>>> any(a)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: The truth value of an array with more than one element is ambiguous. Use a.any() or a.all()
```

numpy 2.0:

```
>>> import numpy as np
>>> a = np.array([[False, False], [False, False]])
>>> np.any(a)
np.False_
>>> any(a)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: The truth value of an array with more than one element is ambiguous. Use a.any() or a.all()
```

Probably the same for `numpy.alltrue`.

---

_Renamed from "NPY201: replacing `sometrue`/`alltrue` with `any`/`all` results in invalid code" to "NPY201: replacing `sometrue`/`alltrue` with `any`/`all` results in broken code" by @radoering on 2024-07-20 05:43_

---

_Label `bug` added by @charliermarsh on 2024-07-20 15:46_

---

_Label `fixes` added by @charliermarsh on 2024-07-20 15:46_

---

_Comment by @charliermarsh on 2024-07-20 15:47_

@mtsokol -- Do you see any issue with this change?

---

_Closed by @MichaReiser on 2024-07-23 08:34_

---

_Comment by @mtsokol on 2024-07-23 09:21_

Right, it's clearly a mistake. It should use NumPy's functions `np.any` and `np.all`. Fixed in https://github.com/astral-sh/ruff/pull/12473. 

---
