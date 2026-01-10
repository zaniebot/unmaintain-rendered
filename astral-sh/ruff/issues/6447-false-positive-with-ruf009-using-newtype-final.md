```yaml
number: 6447
title: False positive with RUF009 using NewType, Final and dataclass
type: issue
state: closed
author: inkhey
labels:
  - bug
  - type-inference
assignees: []
created_at: 2023-08-09T12:22:36Z
updated_at: 2025-01-20T14:44:48Z
url: https://github.com/astral-sh/ruff/issues/6447
synced_at: 2026-01-10T11:09:48Z
```

# False positive with RUF009 using NewType, Final and dataclass

---

_Issue opened by @inkhey on 2023-08-09 12:22_

Hello :wave: , I think I found a false positive on `RUF009` rule. 

test_ruff.py:
```python
from dataclasses import dataclass
from typing import Final, NewType

State = NewType("State", str)


@dataclass
class System:
    state: Final[State] = State("Enabled")


@dataclass
class System2:
    state: Final[str] = "Enabled"
```
version:
```bash
❯ ruff --version
ruff 0.0.283
```
bug.toml:
```toml
select = ["RUF"]
```
output:
```bash
❯ ruff check --config=bug.toml test_ruff.py 
test_ruff.py:9:27: RUF009 Do not perform function call `State` in dataclass defaults
Found 1 error.
```



---

_Comment by @zanieb on 2023-08-09 17:54_

Thanks for the report! We could special case this by looking up the binding for `State` and checking it it's a `NewType` declaration. However, this will not work across files, with aliased assignments, etc. because we do not have type inference.

---

_Label `bug` added by @zanieb on 2023-08-09 17:55_

---

_Label `type-inference` added by @zanieb on 2023-08-09 17:55_

---

_Closed by @dhruvmanila on 2025-01-20 14:44_

---
