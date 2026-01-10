```yaml
number: 275
title: "No `Literal` type narrowing in function body"
type: issue
state: closed
author: janosh
labels: []
assignees: []
created_at: 2025-05-08T15:31:10Z
updated_at: 2025-08-05T13:59:15Z
url: https://github.com/astral-sh/ty/issues/275
synced_at: 2026-01-10T02:06:24Z
```

# No `Literal` type narrowing in function body

---

_Issue opened by @janosh on 2025-05-08 15:31_

### Summary

`ty check ty_repro.py`

```
warning: lint:possibly-unbound-attribute: Attribute `removeprefix` on type `@Todo | None` is possibly unbound
  --> tmp/ty.py:18:16
   |
16 |     if handle_dup_idx in ("keep-first", "keep-last"):
17 |         keep = handle_dup_idx.removeprefix("keep-")
   |                ^^^^^^^^^^^^^^^^^^^^^^^^^^^
19 |         return df_dummy[~df_dummy.index.duplicated(keep=keep)]
20 |     return None
   |
info: `lint:possibly-unbound-attribute` is enabled by default

Found 1 diagnostic
```

```py
"""minimal repro for ty not type-narrowing in if statement."""

from __future__ import annotations

from typing import Literal

import pandas as pd

HandleDupes = Literal["keep-first", "keep-last", "mean", None]


df_dummy = pd.DataFrame({"A": [1, 2, 3, 4, 5]})


def test(handle_dup_idx: HandleDupes = None):
    if handle_dup_idx in ("keep-first", "keep-last"):
        # get possibly-unbound-attribute on next line
        keep = handle_dup_idx.removeprefix("keep-")
        return df_dummy[~df_dummy.index.duplicated(keep=keep)]
    return None


handle_dup_idx: HandleDupes = None

if handle_dup_idx in ("keep-first", "keep-last"):
    keep = handle_dup_idx.removeprefix("keep-")
    # no possibly-unbound-attribute here
    df_dummy = df_dummy[~df_dummy.index.duplicated(keep=keep)]
```

### Version

ty 0.0.0-alpha.7 (905a3e1e5 2025-05-07)

---

_Comment by @carljm on 2025-05-08 15:36_

The issue here is not type narrowing support, but implicit type alias support. If the `Literal` type is used directly in the function annotation, it works: https://play.ty.dev/2cdc0c3f-9e0f-4f96-8274-5a3239ddf362

Or if an explicit PEP 695 type alias is used: https://play.ty.dev/81f847ad-aa2e-44ba-9b97-d043ce1469fc

---

_Comment by @carljm on 2025-05-08 15:39_

I added this specific case to #221 and will close this as duplicate. Thank you for the report!

---

_Closed by @carljm on 2025-05-08 15:39_

---

_Comment by @AKuederle on 2025-08-05 13:07_

> The issue here is not type narrowing support, but implicit type alias support. If the `Literal` type is used directly in the function annotation, it works: https://play.ty.dev/2cdc0c3f-9e0f-4f96-8274-5a3239ddf362
> 
> Or if an explicit PEP 695 type alias is used: https://play.ty.dev/81f847ad-aa2e-44ba-9b97-d043ce1469fc

The direct annotation only partially works I think. If we add any other Union value to the Literal the type inference breaks (e.g. `Literal["keep-first", "keep-last", "mean", None] | int`):

https://play.ty.dev/76032273-7067-4f11-97b6-2e062f6b6e48



---

_Comment by @carljm on 2025-08-05 13:34_

> If we add any other Union value to the Literal the type inference breaks

This is intentional. The type `int` includes instances of user subclasses of `int`, which may override `__eq__` to compare equal to anything. So the test `handle_dup_idx in ("keep-first", "keep-last")` cannot rule out the possibility of the type `int`.

It does look like we could be a bit less conservative here; although we can't rule out `int`, we should still be able to rule out `"mean"` and `None`, and currently we don't even eliminate those, if any type is present whose equality-test results we aren't sure of. So the preferable type should be `Literal["keep-first", "keep-last"] | int`. I created https://github.com/astral-sh/ty/issues/939 to track this.

---

_Comment by @AKuederle on 2025-08-05 13:59_

Oh true! I did not think about the possibility that `__eq__` might be modified. Thanks for the fast response :) Awesome work with ty!

---
