---
number: 10601
title: "`DTZ007` with F-string format specifier"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - rule
assignees: []
created_at: 2024-03-26T07:45:41Z
updated_at: 2024-03-29T04:27:14Z
url: https://github.com/astral-sh/ruff/issues/10601
synced_at: 2026-01-10T01:22:50Z
---

# `DTZ007` with F-string format specifier

---

_Issue opened by @MichaReiser on 2024-03-26 07:45_

              Don't know if this is the same issue, but there is also a false-positive if the format string is an f-string.

For example:
```python
from datetime import datetime
def parse_iso(iso_str,millis=True):
    return datetime.strptime(iso_str, f"%Y-%m-%dT%H:%M:%S{('.%f' if millis else '')}%z")
```

Even without a condition inside the f-string, it immediately triggers the lint error.

For example:
```python
from datetime import datetime
dt = datetime.strptime("","%Y-%m-%d %H:%M:%S%z") # everything okay

dt = datetime.strptime("", f"%Y-%m-%d %H:%M:%S%z") # throws DTZ007
```

_Originally posted by @jonas-w in https://github.com/astral-sh/ruff/issues/1306#issuecomment-2018928777_
            

---

_Label `bug` added by @MichaReiser on 2024-03-26 07:45_

---

_Label `rule` added by @MichaReiser on 2024-03-26 07:45_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-03-29 03:18_

---

_Referenced in [astral-sh/ruff#10651](../../astral-sh/ruff/pulls/10651.md) on 2024-03-29 03:21_

---

_Closed by @dhruvmanila on 2024-03-29 04:27_

---
