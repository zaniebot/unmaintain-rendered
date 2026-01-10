```yaml
number: 6267
title: "RUF012 should allow plain `ClassVar`"
type: issue
state: closed
author: bluetech
labels: []
assignees: []
created_at: 2023-08-02T08:03:19Z
updated_at: 2023-08-02T12:58:46Z
url: https://github.com/astral-sh/ruff/issues/6267
synced_at: 2026-01-10T11:09:48Z
```

# RUF012 should allow plain `ClassVar`

---

_Issue opened by @bluetech on 2023-08-02 08:03_

Running ruff `0.0.282` using `ruff --isolated --select=RUF012` on the following file:

```py
from typing import ClassVar

class C:
    f: ClassVar = {0}
```

gives

```
x.py:4:19: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
```

I think Ruff should not emit RUF012 for this, since if `ClassVar` is used without the `[...]` part, the type checkers infer the type, i.e. the `[...]` part is optional, similar to `Final`.

---

_Closed by @charliermarsh on 2023-08-02 12:58_

---
