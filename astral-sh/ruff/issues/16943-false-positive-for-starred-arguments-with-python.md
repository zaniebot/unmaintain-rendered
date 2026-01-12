```yaml
number: 16943
title: False positive for starred arguments with Python 3.10
type: issue
state: closed
author: je-cook
labels:
  - bug
assignees: []
created_at: 2025-03-24T08:33:08Z
updated_at: 2025-03-24T14:36:06Z
url: https://github.com/astral-sh/ruff/issues/16943
synced_at: 2026-01-12T15:54:55Z
```

# False positive for starred arguments with Python 3.10

---

_@je-cook_

### Summary

I was trying to update my local ruff to 0.11.2 and I ran into a false positive for a syntax with starred expansion on Python 3.10. Below is a minimal example that raised the error `SyntaxError: Cannot use star expression in index on Python 3.10 (syntax was added in Python 3.11)`:

```python
Python 3.10.10 (main, Mar 28 2023, 14:51:24) [GCC 11.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy as np
>>> ind = (0, 1)
>>> out = np.zeros((2, 2, 2, 3))
>>> out[(*(slice(None) for _ in range(2)), *ind)] = 1
>>> out
array([[[[0., 1., 0.],
         [0., 0., 0.]],

        [[0., 1., 0.],
         [0., 0., 0.]]],


       [[[0., 1., 0.],
         [0., 0., 0.]],

        [[0., 1., 0.],
         [0., 0., 0.]]]])
```

Thanks!

### Version

ruff 0.11.2

---

_Label `bug` added by @ntBre on 2025-03-24 11:38_

---

_Assigned to @ntBre by @ntBre on 2025-03-24 11:38_

---

_Comment by @ntBre on 2025-03-24 13:51_

Thanks for the report! It looks like *unparenthesized* starred arguments are new in 3.11, but with parentheses it's okay in 3.10 too.

```pycon
>>> out[(*(slice(None) for _ in range(2)), *ind)]
array([[1., 1.],
       [1., 1.]])
>>> out[*(slice(None) for _ in range(2)), *ind]
  File "<stdin>", line 1
    out[*(slice(None) for _ in range(2)), *ind]
        ^
SyntaxError: invalid syntax
```

---

_Closed by @ntBre on 2025-03-24 14:34_

---

_Closed by @ntBre on 2025-03-24 14:34_

---

_Comment by @je-cook on 2025-03-24 14:36_

Thanks very much!

---
