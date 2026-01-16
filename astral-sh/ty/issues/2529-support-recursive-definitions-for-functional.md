```yaml
number: 2529
title: Support recursive definitions for functional namedtuples
type: issue
state: open
author: charliermarsh
labels: []
assignees: []
created_at: 2026-01-16T03:03:28Z
updated_at: 2026-01-16T03:06:02Z
url: https://github.com/astral-sh/ty/issues/2529
synced_at: 2026-01-16T04:05:13Z
```

# Support recursive definitions for functional namedtuples

---

_@charliermarsh_

E.g., Mypy and Pyright both support this:
```python
from typing import NamedTuple

NT = NamedTuple("NT", [("field", "NT | int")])

NT(1)
NT(NT(0))
NT("foo")  # error
```


---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-16 03:06_

---
