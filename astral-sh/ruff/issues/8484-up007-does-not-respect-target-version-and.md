```yaml
number: 8484
title: "UP007: Does not respect target version and suggests incompatible syntax"
type: issue
state: closed
author: chbndrhnns
labels: []
assignees: []
created_at: 2023-11-04T09:01:19Z
updated_at: 2023-11-04T13:30:28Z
url: https://github.com/astral-sh/ruff/issues/8484
synced_at: 2026-01-10T11:09:50Z
```

# UP007: Does not respect target version and suggests incompatible syntax

---

_Issue opened by @chbndrhnns on 2023-11-04 09:01_

To me it seems that ruff should only suggest fixes that are compatible with the configured target version. That's not the case for this example using ruff 0.1.4.

Command:

```
ruff check .
```

Config:

```
target-version = "py39"

[lint]
select = ["UP"]

```

Code:

```py

from __future__ import annotations

from typing import Optional


class C:
    int: Optional[int]

```

Output:
```
main.py:7:10: UP007 Use `X | Y` for type annotations
```

This is even auto-fixable but will result in a syntax error on Python 3.9

---

_Renamed from "UP007: Suggests incompatible syntax" to "UP007: Does not respect target version and suggests incompatible syntax" by @chbndrhnns on 2023-11-04 09:01_

---

_Comment by @tjkuson on 2023-11-04 10:04_

Using `|` isn't a syntax error in Python 3.9 if you import `annotations` from `__future__`. More information can be found on the [docs](https://docs.astral.sh/ruff/rules/non-pep604-annotation/).

---

_Comment by @chbndrhnns on 2023-11-04 11:55_

Thanks, I read the docs now and it turned out it's coming from pydantic. Thanks much!

---

_Closed by @chbndrhnns on 2023-11-04 11:55_

---

_Comment by @charliermarsh on 2023-11-04 13:30_

👍 You can also disable this behavior (of using newer syntax when `__future__` is present via [`keep-runtime-typing`](https://docs.astral.sh/ruff/settings/#pyupgrade-keep-runtime-typing).

---
