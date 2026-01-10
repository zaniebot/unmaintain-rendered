```yaml
number: 6236
title: "Don't move imports under `if TYPE_CHECKING` if they're added to `__all__`"
type: issue
state: closed
author: pawamoy
labels: []
assignees: []
created_at: 2023-08-01T11:51:04Z
updated_at: 2023-08-01T15:04:43Z
url: https://github.com/astral-sh/ruff/issues/6236
synced_at: 2026-01-10T11:09:48Z
```

# Don't move imports under `if TYPE_CHECKING` if they're added to `__all__`

---

_Issue opened by @pawamoy on 2023-08-01 11:51_

In my top-level `__init__.py` file, I define the `__all__` variable to mark some imported objects as "public" or "re-exported".

```python
from __future__ import annotations

from pkg.submodule import A, B, C

__all__: list[str] = ["A", "B", "C"]
```

When auto-fixing this code with Ruff 0.0.281, Ruff moves all imports under an `if TYPE_CHECKING` condition, probably because they appear unused, and because I import `annotations` from `__future__ `. If I don't import future annotations, Ruff does not refactor the code (I'm targeting Python 3.8).

Moving these public imports under the `TYPE_CHECKING` condition actually breaks the API, since the objects are not available from the top-level module anymore at runtime.

I would like Ruff to check if an object is added to the module's `__all__` list before moving it under `TYPE_CHECKING`.

This `__all__` variable is a sequence of strings, tuple or list, and can unfortunately be constructed dynamically, so this complicates things: `__all__ = ["A", *other_list] + list(some_tuple)`. If supporting dynamically constructed lists is too hard, for a first version Ruff could only support statically defined lists/tuples.

In the meantime, and as explained above, I can simply stop importing future annotations, and Ruff will not refactor anything.


---

_Comment by @charliermarsh on 2023-08-01 13:11_

I believe this is fixed on `main` and will be out in a new release shortly.

---

_Closed by @charliermarsh on 2023-08-01 13:11_

---

_Comment by @pawamoy on 2023-08-01 13:56_

Incredible, thank you so much!

---

_Comment by @charliermarsh on 2023-08-01 15:04_

This should be fixed in v0.0.282, which is out now.

---
