```yaml
number: 2529
title: Support recursive definitions for functional namedtuples
type: issue
state: closed
author: charliermarsh
labels:
  - typing semantics
  - namedtuples
assignees: []
created_at: 2026-01-16T03:03:28Z
updated_at: 2026-01-21T16:03:58Z
url: https://github.com/astral-sh/ty/issues/2529
synced_at: 2026-01-21T17:03:34Z
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

_Label `namedtuples` added by @AlexWaygood on 2026-01-16 09:11_

---

_Added to milestone `Stable` by @AlexWaygood on 2026-01-16 09:11_

---

_Label `typing semantics` added by @AlexWaygood on 2026-01-16 09:12_

---

_Closed by @AlexWaygood on 2026-01-21 16:03_

---
