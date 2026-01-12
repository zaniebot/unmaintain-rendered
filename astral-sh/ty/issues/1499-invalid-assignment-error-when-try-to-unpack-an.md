```yaml
number: 1499
title: invalid-assignment error when try to unpack an array shape when the array has type hint with multiple shape
type: issue
state: closed
author: BaoNguyen6742
labels:
  - narrowing
assignees: []
created_at: 2025-11-07T08:39:53Z
updated_at: 2025-11-07T14:01:03Z
url: https://github.com/astral-sh/ty/issues/1499
synced_at: 2026-01-12T15:54:25Z
```

# invalid-assignment error when try to unpack an array shape when the array has type hint with multiple shape

---

_@BaoNguyen6742_

### Summary

Example code

```
import numpy as np

def get_shape(
    example_image: np.ndarray[
        tuple[int, int, int] | tuple[int, int, int, int], np.dtype[np.float32]
    ],
):
    img_shape = example_image.shape
    img_ndims = example_image.ndim
    if img_ndims == 3:
        h, w, c = img_shape
        n = 1
    else:
        n, h, w, c = img_shape
    return n, h, w, c

```

Error when running `ty check`

```
error[invalid-assignment]: Too many values to unpack
  --> ty_test.py:11:9
   |
 9 |     img_ndims = example_image.ndim
10 |     if img_ndims == 3:
11 |         h, w, c = img_shape
   |         ^^^^^^^   --------- Got 4
   |         |
   |         Expected 3
12 |         n = 1
13 |     else:
   |
info: rule `invalid-assignment` is enabled by default

error[invalid-assignment]: Not enough values to unpack
  --> ty_test.py:14:9
   |
12 |         n = 1
13 |     else:
14 |         n, h, w, c = img_shape
   |         ^^^^^^^^^^   --------- Got 3
   |         |
   |         Expected 4
15 |     return n, h, w, c
   |
info: rule `invalid-assignment` is enabled by default

Found 2 diagnostics
```

Is there any way to make the `img_shape` know that the array has the correct number of dimensions when unpack so it doesn't resulted in this type of error

### Version

ty 0.0.1-alpha.25 (3abd4c968 2025-10-29)

---

_Label `library` added by @sharkdp on 2025-11-07 13:55_

---

_Label `library` removed by @sharkdp on 2025-11-07 13:58_

---

_Label `narrowing` added by @sharkdp on 2025-11-07 13:58_

---

_Comment by @sharkdp on 2025-11-07 14:01_

Thank you for reporting this. No other type checker supports checking this code in the current form, because the length of the shape tuple is stored in `img_ndims`, instead of being used in the `if` checks directly. Other type checkers *do* however support narrowing if it is rewritten as:
```py
import numpy as np


def get_shape(
    example_image: np.ndarray[
        tuple[int, int, int] | tuple[int, int, int, int],
        np.dtype[np.float32]
    ],
):
    img_shape = example_image.shape

    if len(img_shape) == 3:
        h, w, c = img_shape
    elif len(img_shape) == 4:
        b, h, w, c = img_shape
    else:
        raise ValueError(f"Unexpected number of dimensions: {len(img_shape)}")
```

We also plan to add support for this, please see https://github.com/astral-sh/ty/issues/560

---

_Closed by @sharkdp on 2025-11-07 14:01_

---
