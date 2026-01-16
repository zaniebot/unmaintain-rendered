```yaml
number: 2528
title: Support string annotations in functional class annotations
type: issue
state: open
author: charliermarsh
labels: []
assignees: []
created_at: 2026-01-16T03:00:32Z
updated_at: 2026-01-16T03:06:03Z
url: https://github.com/astral-sh/ty/issues/2528
synced_at: 2026-01-16T04:05:13Z
```

# Support string annotations in functional class annotations

---

_@charliermarsh_

E.g., Mypy and Pyright both support this:
```python
from typing import NamedTuple

NT = NamedTuple("NT", [("field", "set[int]")])

NT(1)  # error: Argument 1 to "NT" has incompatible type "int"; expected "set[int]"  [arg-type]
```

Today, ty errors on `"set[int]"` with:

```
Object of type `Literal["set[int]"]` is not valid as a `NamedTuple` field type(invalid-type-form)
```

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-16 03:06_

---
