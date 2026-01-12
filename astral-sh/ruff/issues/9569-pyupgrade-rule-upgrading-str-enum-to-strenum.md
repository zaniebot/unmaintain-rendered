```yaml
number: 9569
title: "`pyupgrade` rule: upgrading `str, Enum` to `StrEnum`"
type: issue
state: closed
author: jamesbraza
labels:
  - good first issue
  - rule
assignees: []
created_at: 2024-01-17T21:03:08Z
updated_at: 2024-04-06T01:56:29Z
url: https://github.com/astral-sh/ruff/issues/9569
synced_at: 2026-01-12T15:54:49Z
```

# `pyupgrade` rule: upgrading `str, Enum` to `StrEnum`

---

_@jamesbraza_

Since Python 3.11 there are now `StrEnum` and `IntEnum`:

```python
from enum import Enum, IntEnum, StrEnum

class Foo(str, Enum):  ...  # Python<3.11
class Foo(StrEnum):  ...  # Python>=3.11

class Bar(int, Enum): ...  # Python<3.11
class Bar(IntEnum): ...  # Python>=3.11
```

`pyupgrade` can be used to suggest this change.

However, this request was rejected in `pyupgrade` itself here: https://github.com/asottile/pyupgrade/issues/779. Apparently they're not identical in behavior. Luckily, `ruff` has a concept of safe vs unsafe, so I think `ruff` can actually implement this rule.

The request here is for `ruff` to suggest this upgrade as an unsafe fix, ideally with autofix support.

---

_Comment by @zanieb on 2024-01-17 21:10_

Sounds good to me!

---

_Label `good first issue` added by @zanieb on 2024-01-17 21:10_

---

_Label `rule` added by @zanieb on 2024-01-17 21:10_

---

_Comment by @johnslavik on 2024-01-17 22:20_

Side note here: `IntEnum` has already been available since Python 3.4 and `enum` being introduced. Only `StrEnum` came in 3.11.

---

_Comment by @arferreira on 2024-01-20 16:56_

Hey @zanieb, is this issue active? Can I try to upgrade this?

---

_Comment by @charliermarsh on 2024-01-20 17:42_

Go for it @arferreira!

---

_Assigned to @arferreira by @charliermarsh on 2024-01-20 17:42_

---

_Comment by @dhruvmanila on 2024-02-12 06:06_

Relevant discussion: https://github.com/astral-sh/ruff/discussions/3867

---

_Closed by @charliermarsh on 2024-04-06 01:56_

---
