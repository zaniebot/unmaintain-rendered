---
number: 4044
title: ruff --fix removes import which is needed
type: issue
state: closed
author: Qumeric
labels:
  - bug
assignees: []
created_at: 2023-04-20T11:37:50Z
updated_at: 2023-04-21T01:37:39Z
url: https://github.com/astral-sh/ruff/issues/4044
synced_at: 2026-01-07T13:12:14-06:00
---

# ruff --fix removes import which is needed

---

_Issue opened by @Qumeric on 2023-04-20 11:37_

I am running `ruff --fix` with the default config and it removes an import which is needed.

```
# observation_log.py
from __future__ import annotations
from dataclasses import dataclass, field
from typing import TYPE_CHECKING
from game_time import current_datetime

if TYPE_CHECKING:
    from datetime import datetime

@dataclass
class Observation:
    datetime: datetime = field(default_factory=current_datetime)
```

```
# game_time.py
from datetime import datetime, timedelta

_ticks = 0
def current_datetime() -> datetime:
    return datetime(1, 1, 1) + timedelta(minutes=_ticks)

```

Ruff removes `datetime` import. It is clearly used and also `mypy` reports it as an error `[name-defined]`.

Moving it from `TYPE_CHECKING` to the top level didn't help either.

v0.0.262

---

_Comment by @Qumeric on 2023-04-20 11:43_

Oh, I guess that my code is problematic, I didn't notice I named my variable `datetime`, the same name as the other `datetime` I use.

However, it works fine in Python (I think) and the current behaviour is not ideal.

FWIW when I added `# noqa` to the import line, I got 
`components/observation_log.py:28:5: F811 Redefinition of unused 'datetime' from line 18`

---

_Comment by @charliermarsh on 2023-04-21 01:37_

Ah yeah, I agree that this is a bug, though I think it's a duplicate of an old issue (https://github.com/charliermarsh/ruff/issues/1401), so I'm going to mark it as a duplicate. Let me know if that seems off.

---

_Closed by @charliermarsh on 2023-04-21 01:37_

---

_Label `bug` added by @charliermarsh on 2023-04-21 01:37_

---

_Comment by @charliermarsh on 2023-04-21 01:37_

Funnily enough it's always `datetime` shadowing `datetime`!

---
