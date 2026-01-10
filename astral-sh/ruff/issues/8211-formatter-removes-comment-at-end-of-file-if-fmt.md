```yaml
number: 8211
title: "Formatter removes comment at end of file if `fmt: off` is never closed"
type: issue
state: closed
author: tmke8
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-10-25T11:42:27Z
updated_at: 2023-10-26T01:03:36Z
url: https://github.com/astral-sh/ruff/issues/8211
synced_at: 2026-01-10T11:09:50Z
```

# Formatter removes comment at end of file if `fmt: off` is never closed

---

_Issue opened by @tmke8 on 2023-10-25 11:42_

```python
# fmt: off
from dataclasses import dataclass

@dataclass
class A:
    x: int # Optional[int]
```

is formatted to this:
```python
# fmt: off
from dataclasses import dataclass

@dataclass
class A:
    x: int
```

playground: https://play.ruff.rs/48837d60-4eb8-4c2e-a007-1bc764cb6735



---

_Label `bug` added by @MichaReiser on 2023-10-25 12:56_

---

_Label `formatter` added by @MichaReiser on 2023-10-25 12:56_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-25 12:56_

---

_Comment by @charliermarsh on 2023-10-25 17:59_

Thanks for filing!

---

_Assigned to @MichaReiser by @MichaReiser on 2023-10-26 00:11_

---

_Closed by @MichaReiser on 2023-10-26 01:03_

---
