---
number: 9053
title: False positive runtime-import-in-type-checking-block (TCH004)
type: issue
state: closed
author: jschippergoauto
labels: []
assignees: []
created_at: 2023-12-08T16:30:14Z
updated_at: 2023-12-08T17:05:20Z
url: https://github.com/astral-sh/ruff/issues/9053
synced_at: 2026-01-07T13:12:15-06:00
---

# False positive runtime-import-in-type-checking-block (TCH004)

---

_Issue opened by @jschippergoauto on 2023-12-08 16:30_

Ruff (0.1.7) gives me a false positive TCH004.

Code:
```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from sqlalchemy.orm import Query


def x(q: Query) -> None:
    pass
```

Output of `ruff --isolated --select TCH test.py`:
```
test.py:4:32: TCH004 Move import `sqlalchemy.orm.Query` out of type-checking block. Import is used for more than type hinting.
Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

---

_Comment by @charliermarsh on 2023-12-08 16:32_

This is actually correct -- Python requires that type annotations on functions are available at runtime. (If you try to run the above code, you'll get a runtime error.) If you add `from __future__ import annotations`, or quote the `"Query"`, Ruff should stop warning you.

---

_Comment by @jschippergoauto on 2023-12-08 16:53_

@charliermarsh Nice, I did not know that! I thought they were not needed at runtime at all. Ruff could be advertised not just as a linter/formatter but as an educational tool as well :laughing:. Thanks!

---

_Closed by @jschippergoauto on 2023-12-08 16:53_

---

_Comment by @charliermarsh on 2023-12-08 17:00_

Haha, I learned this too through implementing the rule :joy: I was surprised!

There are a few exceptions... for example, annotated assignments outside of the top-level, e.g. here the annotated isn't required to be available at runtime:

```python
def func():
    x: Query = None
```

(Ruff handles these correctly, or at least it should.)

---

_Comment by @jschippergoauto on 2023-12-08 17:05_

> There are a few exceptions... for example, annotated assignments outside of the top-level, e.g. here the annotated isn't required to be available at runtime:
> 
> ```python
> def func():
>     x: Query = None
> ```
> 
> (Ruff handles these correctly, or at least it should.)

Double checked and yes it works properly there as well :slightly_smiling_face:. Having worked mostly on statically typed languages I'm surprised the way Python works with this. Thanks for clearing up things.

---
